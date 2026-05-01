---
title: "Enabling Service Connect"
weight: 1
---
> **작성일:** 2026-05-01 | **수정일:** 2026-05-01
## Deploy the Orders service

```json
cat << EOF > retail-store-ecs-order-taskdef.json
{
    "family": "retail-store-ecs-orders",
    "executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/ordersEcsTaskExecutionRole",
    "taskRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskRole",
    "networkMode": "awsvpc",
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "cpu": "1024",
    "memory": "2048",
    "runtimePlatform": {
        "cpuArchitecture": "X86_64",
        "operatingSystemFamily": "LINUX"
    },
    "containerDefinitions": [
        {
            "name": "application",
            "image": "public.ecr.aws/aws-containers/retail-store-sample-orders:1.2.3",
            "cpu": 0,
            "portMappings": [
                {
                    "name": "application",
                    "containerPort": 8080,
                    "hostPort": 8080,
                    "protocol": "tcp",
                    "appProtocol": "http"
                }
            ],
            "essential": true,
            "linuxParameters": {
                "initProcessEnabled": true
            },
            "healthCheck": {
                "command": [
                    "CMD-SHELL",
                    "curl -f http://localhost:8080/actuator/health || exit 1"
                ],
                "interval": 10,
                "timeout": 5,
                "retries": 3,
                "startPeriod": 30
            },
            "versionConsistency": "disabled",
            "environment": [
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_PROVIDER",
                    "value": "postgres"
                },
                {
                    "name": "RETAIL_ORDERS_MESSAGING_PROVIDER",
                    "value": "in-memory"
                }
            ],
            "secrets": [
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_ENDPOINT",
                    "valueFrom": "arn:aws:ssm:${AWS_REGION}:${ACCOUNT_ID}:parameter/retail-store-ecs/orders/db-endpoint-postgres"
                },
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_NAME",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:dbname::"
                },
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_USERNAME",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:username::"
                },
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_PASSWORD",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:password::"
                }
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "retail-store-ecs-tasks",
                    "awslogs-region": "$AWS_REGION",
                    "awslogs-stream-prefix": "orders-service"
                }
            }
        }
    ]
}
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-order-taskdef.json
```

Orders 서비스의 Task Definition입니다.

Orders 서비스는 ECS Service로 실행됩니다. ECS Service는 Orders 애플리케이션을 실행하는 Task들을 관리합니다. 이 Task들은 실행 과정에서 특정 권한을 필요로 합니다. ECS/Fargate가 Task를 실행하기 위한 권한은 `"executionRoleArn"`으로 지정하고, Task 내부 컨테이너에서 실행되는 애플리케이션이 AWS 리소스에 접근하기 위한 권한은 `"taskRoleArn"`으로 지정합니다.

---

`"containerDefinitions"`에 `"cpu": 0`은 이 컨테이너에 CPU를 따로 지정하지 않았다는 뜻입니다. 태스크에 할당된 CPU 안에서 필요에 따라 CPU를 사용합니다.

---

```json
            "environment": [
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_PROVIDER",
                    "value": "postgres"
                },
                {
                    "name": "RETAIL_ORDERS_MESSAGING_PROVIDER",
                    "value": "in-memory"
                }
            ],
```

`"environment"`는 컨테이너에 전달할 일반 환경 변수를 정의합니다. 

여기서는 Orders 애플리케이션이 주문 데이터를 PostgreSQL에 저장하도록 `"RETAIL_ORDERS_PERSISTENCE_PROVIDER"` 값을 `"postgres"`로 설정합니다. 또한 메시징 방식은 외부 메시지 브로커를 사용하지 않고 애플리케이션 메모리 안에서 처리하도록 `"RETAIL_ORDERS_MESSAGING_PROVIDER"` 값을 `"in-memory"`로 설정합니다. 

여기서 메시징은 Orders 서비스가 주문 생성과 같은 이벤트를 다른 기능에 전달하는 방식을 의미합니다. `"in-memory"`는 SQS나 Kafka 같은 외부 메시지 시스템을 사용하지 않고, 애플리케이션 내부 메모리에서 간단히 처리한다는 뜻입니다.

메시지는 “어떤 일이 발생했다”는 알림 데이터

메시지 예시:

```json
{
  "eventType": "OrderCreated",
  "orderId": "order-123"
}
```

`orders`는 주문 관련 메시지를 외부 큐나 브로커로 보내지 않고, 실행 중인 `orders` 애플리케이션 안에서 바로 처리합니다. 즉, 자기 애플리케이션 내부 로직에서 사용할 수 있다는 뜻입니다.

예를 들면:

```
주문 생성 요청 들어옴
→ orders가 주문을 DB에 저장
→ "OrderCreated" 메시지/이벤트를 만듦
→ orders 내부의 다른 로직이 그 이벤트를 받아 후속 처리
```

---

```json
            "secrets": [
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_ENDPOINT",
                    "valueFrom": "arn:aws:ssm:${AWS_REGION}:${ACCOUNT_ID}:parameter/retail-store-ecs/orders/db-endpoint-postgres"
                },
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_NAME",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:dbname::"
                },
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_USERNAME",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:username::"
                },
                {
                    "name": "RETAIL_ORDERS_PERSISTENCE_PASSWORD",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:password::"
                }
            ],
```

`"secrets"`는 컨테이너에 민감한 설정값을 환경 변수로 주입하는 설정입니다. 

`"environment"`와 달리 값을 Task Definition에 직접 작성하지 않고, SSM Parameter Store나 Secrets Manager에 저장된 값을 가져와 컨테이너에 전달합니다.

여기서는 Orders 애플리케이션이 PostgreSQL 데이터베이스에 접속하는 데 필요한 DB 엔드포인트, DB 이름, 사용자 이름, 비밀번호를 환경 변수로 주입합니다. 컨테이너 안에서 실행되는 Orders 애플리케이션은 이 환경 변수를 읽어 데이터베이스에 연결합니다.

>`dbname` 자체는 비밀번호처럼 강한 기밀 정보는 아니지만, DB 접속에 필요한 설정값들을 하나의 Secret으로 묶어 관리하기 위해 Secrets Manager에 함께 저장될 수 있습니다. 여기서는 `retail-store-ecs-orders-db` Secret 안에서 `dbname` 필드만 가져와 컨테이너 환경 변수로 주입합니다.

> DB 엔드포인트, 사용자 이름, 사용자 비밀번호는 민감한 정보로 관리하는 것이 좋습니다. DB 엔드포인트가 노출되면 DB에 접근할 수 있는 경로가 유출될 수 있고, 사용자 이름과 비밀번호가 함께 유출되면 실제 DB 접속에 악용될 수 있습니다. 따라서 이러한 값들은 Task Definition에 직접 작성하지 않고, SSM Parameter Store나 Secrets Manager에 저장한 뒤 컨테이너 실행 시 환경 변수로 주입합니다.

---

```bash
aws ecs create-service \
    --cluster retail-store-ecs-cluster \
    --service-name orders \
    --task-definition retail-store-ecs-orders \
    --desired-count 1 \
    --launch-type FARGATE \
    --health-check-grace-period-seconds 30 \
    --enable-execute-command \
    --network-configuration "awsvpcConfiguration={subnets=[${PRIVATE_SUBNET1}, ${PRIVATE_SUBNET2}], securityGroups=[$ORDERS_SG_ID],assignPublicIp=DISABLED}" \
    --service-connect-configuration '{
        "enabled": true,
        "namespace": "retailstore.local",
        "services": [
            {
                "portName": "application",
                "discoveryName": "orders",
                "clientAliases": [
                    {
                        "port": 80,
                        "dnsName": "orders"
                    }
                ]
            }
        ]
    }'
```

위 명령어는 기존 ECS 클러스터 안에 Orders 라는 ECS 서비스를 새로 생성하는 명령어입니다. 
UI 서비스와 Orders 서비스의 명령어를 비교해보겠습니다. 

---

`--desired-count 1`을 보면, `orders` 서비스가 유지하려는 태스크 수는 1개임을 알 수 있습니다. 반면 `ui` 서비스는 이 값이 2였습니다. `ui` 서비스는 로드 밸런서를 통해 사용자 요청을 직접 받는 입구 역할을 하므로, 더 많은 요청을 처리하고 가용성을 높이기 위해 태스크 수를 더 많이 설정한 것으로 볼 수 있습니다.

---

`orders` 서비스는 로드 밸런서로부터 직접 트래픽을 받는 서비스가 아니라, `ui` 서비스 같은 내부 서비스가 Service Connect를 통해 호출하는 백엔드 서비스입니다. 따라서 `orders` 서비스에는 로드 밸런서 옵션이 없습니다.

`ui` 서비스는 로드 밸런서에 직접 연결되어 외부 트래픽을 받는 입구 역할을 합니다. 그래서 다른 내부 서비스가 `ui`를 이름으로 찾아 호출해야 하는 상황이 아니라면 Service Connect에 등록하지 않아도 됩니다. 반면 `orders` 서비스는 `ui` 같은 내부 서비스가 `orders`라는 이름으로 호출해야 하므로 Service Connect에 등록되어 있습니다.

Service Connect는 ECS 서비스 간 통신을 쉽게 해주는 기능입니다. 다만 양방향 통신이 필요하다면 양쪽 서비스가 각각 호출 가능한 이름으로 등록되어 있어야 합니다. 현재 구조에서는 `ui`가 `orders`를 호출하는 흐름이 필요하므로 `orders`만 Service Connect에 등록되어 있습니다.

`orders` 서비스에만 ECS Exec이 활성화되어 있습니다. `ui` 서비스는 로드 밸런서로부터 트래픽을 받아 처리하는 입구 역할을 하므로, 컨테이너 내부에 직접 접속해 확인할 필요성이 상대적으로 낮습니다. 반면 `orders` 서비스는 내부 백엔드 서비스로, DB 연결, 환경 변수, 내부 통신 상태 등을 확인해야 할 수 있기 때문에 ECS Exec이 활성화된 것으로 볼 수 있습니다.

요약하자면, `ui` 서비스는 로드 밸런서로부터 외부 트래픽을 받는 입구 역할을 합니다. 사용자의 요청을 직접 처리하고, 필요한 경우 `orders` 같은 내부 백엔드 서비스를 호출합니다. 따라서 이 예제에서는 `ui` 컨테이너 내부에 직접 접속해 점검할 필요성이 상대적으로 낮기 때문에 ECS Exec이 활성화되어 있지 않은 것으로 볼 수 있습니다. 또한 다른 내부 서비스가 `ui`를 이름으로 찾아 호출해야 하는 구조가 아니라면, `ui` 서비스는 Service Connect에 등록되지 않아도 됩니다.

반면 `orders` 서비스는 외부 로드 밸런서에 직접 연결되지 않는 내부 백엔드 서비스입니다. DB와 연결되어 있고, 환경 변수, DB 연결 상태, 내부 통신 상태 등을 확인할 필요성이 상대적으로 크기 때문에 ECS Exec이 활성화된 것으로 볼 수 있습니다. 또한 `ui`나 `checkout` 같은 다른 서비스가 `orders`라는 이름으로 호출해야 하므로 Service Connect에 등록되어 있습니다.

---

```json
    --service-connect-configuration '{
        "enabled": true,
        "namespace": "retailstore.local",
        "services": [
            {
                "portName": "application",
                "discoveryName": "orders",
                "clientAliases": [
                    {
                        "port": 80,
                        "dnsName": "orders"
                    }
                ]
            }
        ]
    }'
```

`--service-connect-configuration`에 지정된 Service Connect 설정 항목을 살펴보겠습니다.

`"enabled": true`는 이 서비스에서 Service Connect를 활성화하겠다는 설정입니다. 이 값이 `true`이면 ECS가 해당 서비스를 Service Connect 구성에 포함시키고, 다른 서비스들이 지정된 DNS 이름으로 이 서비스를 호출할 수 있게 됩니다.

>애플리케이션은 `orders` 같은 DNS 이름으로 호출합니다. 그리고 그 DNS 이름은 내부적으로 실제 IP 주소로 해석되어 통신이 이루어집니다.

`"namespace": "retailstore.local"`은 Service Connect가 서비스 이름을 관리하는 내부 네임스페이스입니다. 이 안에 `orders` 같은 서비스 검색 이름이 등록되고, 다른 서비스는 `orders` 또는 `orders.retailstore.local` 같은 이름으로 해당 서비스를 찾을 수 있습니다.

`"services"`는 Service Connect에 등록할 서비스 목록입니다.

`"portName": "application"`은 Task Definition에 정의된 포트 이름을 가리키는 값입니다.
Order 서비서의 Task Definiton을 보면 이`"portMappings"`설정이 존재합니다.

```json
            "portMappings": [
                {
                    "name": "application",
                    "containerPort": 8080,
                    "hostPort": 8080,
                    "protocol": "tcp",
                    "appProtocol": "http"
                }
            ]
```

즉, Service Connect가 이 서비스의 어떤 포트를 사용할지 지정하는 설정입니다. 

`"discoveryName": "orders"`는 이 서비스를 Service Connect 안에서 `orders`라는 이름으로 등록하겠다는 설정입니다. 이 이름은 `retailstore.local` 네임스페이스 안에서 관리되며, 다른 서비스가 `orders`라는 이름으로 이 서비스를 찾을 수 있게 합니다.

`"clientAliases"`는 다른 서비스가 이 서비스를 호출할 때 사용할 별칭을 정하는 설정입니다. 여기서는 다른 서비스가 `orders` 서비스를 `orders:80`으로 호출할 수 있게 합니다. `"clientAliases"`를 사용하지 않으면 보통 `discoveryName`과 `namespace`가 합쳐진 이름으로 호출합니다. 예를 들어 `orders.retailstore.local`처럼 호출할 수 있습니다.

`"clientAliases"`는 다른 서비스가 이 서비스를 짧고 고정된 이름으로 호출할 수 있게 해주는 별칭 설정입니다. 여기서는 다른 서비스가 `orders:80`으로 호출하면 실제 `orders` 서비스로 연결되도록 설정합니다.

---
## Deploy the Checkout service

```json
cat << EOF > retail-store-ecs-checkout-taskdef.json
{
    "family": "retail-store-ecs-checkout",
    "executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/checkoutEcsTaskExecutionRole",
    "taskRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskRole",
    "networkMode": "awsvpc",
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "cpu": "1024",
    "memory": "2048",
    "runtimePlatform": {
        "cpuArchitecture": "X86_64",
        "operatingSystemFamily": "LINUX"
    },
    "containerDefinitions": [
        {
            "name": "application",
            "image": "public.ecr.aws/aws-containers/retail-store-sample-checkout:1.2.3",
            "portMappings": [
                {
                    "name": "application",
                    "containerPort": 8080,
                    "hostPort": 8080,
                    "protocol": "tcp",
                    "appProtocol": "http"
                }
            ],
            "essential": true,
            "linuxParameters": {
                "initProcessEnabled": true
            },
            "environment": [
                {
                    "name": "RETAIL_CHECKOUT_PERSISTENCE_PROVIDER",
                    "value": "redis"
                },
                 {
                    "name": "RETAIL_CHECKOUT_ENDPOINTS_ORDERS",
                    "value": "http://orders"
                }
            ],
            "secrets": [
                {
                    "name": "RETAIL_CHECKOUT_PERSISTENCE_REDIS_URL",
                    "valueFrom": "arn:aws:ssm:${AWS_REGION}:${ACCOUNT_ID}:parameter/retail-store-ecs/checkout/redis-endpoint"
                }
            ],
            "healthCheck": {
                "command": [
                    "CMD-SHELL",
                    "curl -f http://localhost:8080/health || exit 1"
                ],
                "interval": 10,
                "timeout": 5,
                "retries": 3,
                "startPeriod": 30
            },
            "versionConsistency": "disabled",
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "retail-store-ecs-tasks",
                    "awslogs-region": "$AWS_REGION",
                    "awslogs-stream-prefix": "checkout-service"
                }
            }
        }
    ]
}
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-checkout-taskdef.json
```

Checkout 서비스의 Task Definition입니다. Orders 서비스의 Task Definition과 비교해 보겠습니다.

---

`"executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/checkoutEcsTaskExecutionRole"`를 보면 Checkout 서비스 만의 Execution Role을 사용함을 알 수 있습니다.

---

Orders 서비스와 다르게 `"containerDefinitions"`에 `"cpu": 0` 설정값이 없습니다. 하지만 `"cpu": 0`이 있거나 `"cpu"` 항목이 생략된 경우 모두 컨테이너별 CPU 값을 따로 지정하지 않았다는 의미에 가깝습니다. 따라서 컨테이너는 태스크 수준에서 정한 CPU 범위(`"cpu": "1024"`) 안에서 실행됩니다.

>참고로 `"containerDefinitions"`에 별도의 메모리 설정이 없으면, 컨테이너별 메모리 한도를 따로 지정하지 않은 것입니다. 이 경우 컨테이너는 태스크 수준에서 정한 메모리(`"memory": "2048"`) 범위 안에서 실행됩니다. 단일 컨테이너 태스크라면 해당 컨테이너가 태스크 메모리를 사실상 혼자 사용하고, 여러 컨테이너가 있는 태스크라면 태스크 전체 메모리를 함께 사용합니다.

---

`"environment"` 옵션을 보면 Orders 서비스와 달리 `"RETAIL_ORDERS_MESSAGING_PROVIDER"` 같은 설정 항목이 없습니다. 이는 Checkout 서비스가 주문을 직접 생성하고 처리하는 서비스가 아니라, 체크아웃 과정에서 필요한 주문 생성을 Orders 서비스에 요청하는 역할을 하기 때문입니다. 따라서 Checkout 서비스는 메시징을 직접 사용하지 않고, Orders 서비스를 `HTTP`로 호출합니다.

Checkout 서비스가 Orders 서비스에 보내는 예시 요청

```http
POST http://orders/orders
Content-Type: application/json

{
  "customerId": "customer-123",
  "items": [
    {
      "productId": "product-001",
      "quantity": 2
    }
  ]
}
```

Checkout 서비스는 Orders 서비스를 호출하는 쪽이기 때문에 Orders 서비스의 주소, 즉 엔드포인트가 필요합니다. 반면 Orders 서비스는 실제 주문 생성과 저장을 처리하는 쪽이기 때문에, 주문 관련 메시지를 어떻게 처리할지 정하는 메시징 설정이 필요합니다.

---

`"secrets"` 옵션을 보면, Checkout 서비스의 Redis는 이 예제에서 캐시 저장소로 사용되며, 별도의 인증 정보 없이 엔드포인트만으로 접근하도록 구성되어 있습니다. 따라서 Orders 서비스처럼 DB 이름, 사용자 이름, 비밀번호를 각각 secret으로 주입하지 않고, Redis 접속 주소만 secret으로 전달합니다. 다만 이것이 Redis에 중요한 정보가 전혀 없다는 뜻은 아니며, 이 실습 환경에서 Redis 인증 정보를 별도로 사용하지 않는 구성으로 볼 수 있습니다.

---

Orders 서비스의 헬스 체크 경로(`http://localhost:8080/actuator/health`)와 다르게, Checkout 서비스는 `http://localhost:8080/health`를 헬스 체크 경로로 사용합니다. 이는 서비스마다 애플리케이션 구현 방식과 사용하는 프레임워크가 다르기 때문입니다.

Orders 서비스는 Spring Boot 기반 애플리케이션으로 보이며, Spring Boot에서는 Actuator 기능을 통해 애플리케이션 상태를 확인하는 헬스 체크 엔드포인트를 제공할 수 있습니다. 이때 일반적으로 사용되는 경로가 `/actuator/health`입니다.

반면 Checkout 서비스는 `/health`라는 별도의 헬스 체크 엔드포인트를 사용합니다. 즉, ECS 입장에서는 둘 다 컨테이너 상태를 확인하기 위한 헬스 체크 URL이지만, 각 서비스 애플리케이션이 제공하는 헬스 체크 경로가 다르기 때문에 Task Definition에 지정된 경로도 다르게 설정되어 있습니다.

---

```bash
aws ecs create-service \
    --cluster retail-store-ecs-cluster \
    --service-name checkout \
    --task-definition retail-store-ecs-checkout \
    --desired-count 1 \
    --launch-type FARGATE \
    --health-check-grace-period-seconds 30 \
    --enable-execute-command \
    --network-configuration "awsvpcConfiguration={subnets=[${PRIVATE_SUBNET1}, ${PRIVATE_SUBNET2}], securityGroups=[$CHECKOUT_SG_ID],assignPublicIp=DISABLED}" \
    --service-connect-configuration '{
        "enabled": true,
        "namespace": "retailstore.local",
        "services": [
            {
                "portName": "application",
                "discoveryName": "checkout",
                "clientAliases": [
                    {
                        "port": 80,
                        "dnsName": "checkout"
                    }
                ]
            }
        ]
    }'
```

ECS 클러스터 안에 Checkout 서비스를 새로 생성하는 명령어입니다. Orders 서비스를 생성하는 명령어의 구조와 거의 동일합니다. 

---
## Deploy the Catalog service

```json
cat << EOF > retail-store-ecs-catalog-taskdef.json
{
    "family": "retail-store-ecs-catalog",
    "executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/catalogEcsTaskExecutionRole",
    "taskRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskRole",
    "networkMode": "awsvpc",
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "cpu": "1024",
    "memory": "2048",
    "runtimePlatform": {
        "cpuArchitecture": "X86_64",
        "operatingSystemFamily": "LINUX"
    },
    "containerDefinitions": [
        {
            "name": "application",
            "image": "public.ecr.aws/aws-containers/retail-store-sample-catalog:1.2.3",
            "portMappings": [
                {
                    "name": "application",
                    "containerPort": 8080,
                    "hostPort": 8080,
                    "protocol": "tcp",
                    "appProtocol": "http"
                }
            ],
            "essential": true,
            "linuxParameters": {
                "initProcessEnabled": true
            },
            "environment": [
                {
                    "name": "RETAIL_CATALOG_PERSISTENCE_DB_NAME",
                    "value": "catalog"
                },
                {
                    "name": "RETAIL_CATALOG_PERSISTENCE_PROVIDER",
                    "value": "mysql"
                }
            ],
            "secrets": [
                {
                    "name": "RETAIL_CATALOG_PERSISTENCE_ENDPOINT",
                    "valueFrom": "arn:aws:ssm:${AWS_REGION}:${ACCOUNT_ID}:parameter/retail-store-ecs/catalog/db-endpoint-mysql"
                },
                {
                    "name": "RETAIL_CATALOG_PERSISTENCE_PASSWORD",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-catalog-db:password::"
                },
                {
                    "name": "RETAIL_CATALOG_PERSISTENCE_USER",
                    "valueFrom": "arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-catalog-db:username::"
                }
            ],
            "versionConsistency": "disabled",
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "retail-store-ecs-tasks",
                    "awslogs-region": "${AWS_REGION}",
                    "awslogs-stream-prefix": "catalog-service"
                }
            },
            "healthCheck": {
                "command": [
                    "CMD-SHELL",
                    "curl -f http://localhost:8080/health || exit 1"
                ],
                "interval": 10,
                "timeout": 5,
                "retries": 3,
                "startPeriod": 30
            }
        }
    ]
}
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-catalog-taskdef.json
```


Catalog 서비스의 Task Definition입니다. Orders 서비스의 Task Definition과 유사한 구조를 가지고 있습니다. 

주목할 차이점으로는 Catalog 서비스는 데이터 저장소로 `mysql`을 사용합니다. 반면 Orders 서비스는 데이터 저장소로 PostgreSQL 를 사용합니다. 여기서는 서비스마다 서로 다른 데이터 저장소를 사용할 수 있다는 점을 보여주기 위해 Catalog 서비스는 MySQL, Orders 서비스는 PostgreSQL을 사용하도록 구성된 것으로 볼 수 있습니다.

Catalog 서비스는 DB 이름을 `"RETAIL_CATALOG_PERSISTENCE_DB_NAME": "catalog"` 환경 변수로 직접 지정합니다. 이는 DB 이름이 `catalog`라는 고정값으로 사용되기 때문입니다. 반면 DB 엔드포인트, 사용자 이름, 비밀번호는 배포 환경에 따라 달라질 수 있거나 민감한 접속 정보이므로 `secrets`를 통해 주입합니다.

Orders 서비스는 DB 이름까지 Secrets Manager에서 가져오도록 구성되어 있습니다. 즉, Orders 서비스는 DB 이름, 사용자 이름, 비밀번호를 하나의 secret 안에서 함께 관리하고, DB 엔드포인트는 SSM Parameter Store에서 가져오는 구조입니다. 따라서 두 서비스의 차이는 보안 정책의 차이라기보다, 각 애플리케이션이 DB 설정값을 관리하는 방식의 차이로 볼 수 있습니다.

헬스 체크 경로도 다릅니다. Orders 서비스는 `/actuator/health`로 상태를 확인하고, Catalog 서비스는 `/health`로 상태를 확인합니다.

---

```bash
aws ecs create-service \
    --cluster retail-store-ecs-cluster \
    --service-name catalog \
    --task-definition retail-store-ecs-catalog \
    --desired-count 2 \
    --launch-type FARGATE \
    --health-check-grace-period-seconds 30 \
    --enable-execute-command \
    --network-configuration "awsvpcConfiguration={subnets=[${PRIVATE_SUBNET1}, ${PRIVATE_SUBNET2}], securityGroups=[$CATALOG_SG_ID],assignPublicIp=DISABLED}" \
    --service-connect-configuration '{
        "enabled": true,
        "namespace": "retailstore.local",
        "services": [
            {
                "portName": "application",
                "discoveryName": "catalog",
                "clientAliases": [
                    {
                        "port": 80,
                        "dnsName": "catalog"
                    }
                ]
            }
        ]
    }'
```

ECS 클러스터 안에 Catalog 서비스를 새로 생성하는 명령어입니다. Orders 서비스를 생성하는 명령어의 구조와 거의 동일합니다. 

주목할 차이점으로는, Catalog 서비스의 `--desired-count` 값은 `2`입니다. 이는 ECS가 Catalog 서비스의 태스크를 항상 2개 유지하도록 설정한다는 뜻입니다. Catalog 서비스는 상품 목록이나 상품 상세 정보처럼 사용자가 자주 조회하는 데이터를 제공하는 서비스이므로, 요청을 분산하고 가용성을 높이기 위해 Orders 서비스보다 더 많은 태스크를 실행하도록 설정한 것으로 볼 수 있습니다.

---
## Updating the UI service

```json
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services orders
aws ecs wait services-stable --cluster retail-store-ecs-cluster --services checkout
aws ecs wait services-stable --cluster retail-store-ecs-cluster --services catalog
```

위 명령어로 ECS 서비스 업데이트가 안정화될 때까지 기다립니다.

---

```json
"environment": [
    {
        "name": "RETAIL_UI_ENDPOINTS_ORDERS",
        "value": "http://orders"
    },
    {
        "name": "RETAIL_UI_ENDPOINTS_CHECKOUT",
        "value": "http://checkout"
    },
    {
        "name": "RETAIL_UI_ENDPOINTS_CATALOG",
        "value": "http://catalog"
    }
]
```

UI 서비스가 Orders 서비스, Checkout 서비스, Catalog 서비스를 호출하려면 각 서비스의 내부 주소를 알아야 합니다. 그래서 UI Task Definition에 `RETAIL_UI_ENDPOINTS_ORDERS`, `RETAIL_UI_ENDPOINTS_CHECKOUT`, `RETAIL_UI_ENDPOINTS_CATALOG` 환경 변수를 추가합니다. 이 값들은 Service Connect를 통해 연결되는 내부 주소입니다.

---

ECS 서비스 업데이트 안정화가 끝났다면 `lab-prep connect | jq` 명령어를 사용합니다.

`lab-prep connect | jq` 명령어는 실습에서 제공하는 `lab-prep` 스크립트의 `connect` 단계를 실행합니다. 이 과정에서 `retail-store-ecs-ui-connect-taskdef.json` 파일이 생성되고, 생성된 UI Task Definition 내용이 JSON 형식으로 출력됩니다. 뒤의 `| jq`는 출력된 JSON을 읽기 좋게 정렬해서 보여주는 역할을 합니다.

---

이후
`aws ecs register-task-definition --cli-input-json file://retail-store-ecs-ui-connect-taskdef.json` 명령어를 사용합니다.

이 명령어는 이전에서 생성된 Task Definition 파일(`retail-store-ecs-ui-connect-taskdef.json`)을 사용해 새로운 UI Task Definition revision을 등록합니다. 기존 Task Definition을 직접 수정하는 것이 아니라, 변경된 설정을 가진 새 revision을 생성하는 것입니다.

---

```bash
aws ecs update-service \
    --cluster retail-store-ecs-cluster \
    --service ui \
    --task-definition retail-store-ecs-ui \
    --force-new-deployment \
    --desired-count 2 \
    --service-connect-configuration '{
        "enabled": true,
        "namespace": "retailstore.local",
        "logConfiguration": {
            "logDriver": "awslogs",
            "options": {
                "awslogs-group": "'$SERVICE_CONNECT_LOG_GROUP_NAME'",
                "awslogs-region": "'$AWS_REGION'",
                "awslogs-stream-prefix": "ui-service-connect-logs"
            }
        },
        "accessLogConfiguration": {
            "format": "JSON",
            "includeQueryParameters": "ENABLED"
        },
        "services": [
            {
                "portName": "application",
                "discoveryName": "ui",
                "clientAliases": [
                    {
                        "port": 80,
                        "dnsName": "ui"
                    }
                ]
            }
        ]
    }'

echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

위 명령어를 사용하여 기존 UI 서비스를 업데이트하고고 Service Connect에 등록합니다. 앞에서 살펴본 UI 서비스 생성 명령어와 비교해보겠습니다.

이하는 UI 서비스 생성 명령어입니다.

```bash
aws ecs create-service \
    --cluster retail-store-ecs-cluster \
    --service-name ui \
    --task-definition retail-store-ecs-ui \
    --desired-count 2 \
    --launch-type FARGATE \
    --health-check-grace-period-seconds 30 \
    --load-balancers targetGroupArn=${UI_TG_ARN},containerName=application,containerPort=8080 \
    --network-configuration "awsvpcConfiguration={subnets=[${PRIVATE_SUBNET1},${PRIVATE_SUBNET2}],securityGroups=[${UI_SG_ID}],assignPublicIp=DISABLED}"
```

---

UI 서비스를 업데이트하는 명령어의 `--force-new-deployment` 옵션은 새 배포를 강제로 시작하는 옵션입니다. Service Connect 설정은 이미 실행 중인 기존 Task에 바로 적용되는 것이 아니라, 새로 시작되는 Task에 반영됩니다. 따라서 `--force-new-deployment` 옵션을 사용해 기존 Task를 새 설정이 반영된 새 Task로 교체합니다.

> `--force-new-deployment` 옵션은 현재 실행 중인 태스크를 새로 교체하고 싶을 때 사용합니다. Task Definition이 새 revision으로 바뀐 경우에는 ECS가 새 태스크를 배포하므로 보통 이 옵션이 필요하지 않습니다.
>
>반면 Task Definition은 그대로이고 Service Connect 설정 추가나 ECS Exec 활성화처럼 ECS Service 설정을 변경하는 경우에는, 기존 태스크를 새 설정이 반영된 태스크로 교체하기 위해 `--force-new-deployment`를 사용할 수 있습니다.
>
>정리하면 다음과 같습니다.

```
환경 변수 변경
→ Task Definition 변경
→ 새 Task Definition revision으로 업데이트
→ 보통 --force-new-deployment 없이도 새 배포 발생

Service Connect 설정 추가
→ ECS Service 설정 변경
→ 기존 태스크를 새 설정으로 교체하려면 --force-new-deployment 사용

ECS Exec 활성화
→ ECS Service 설정 변경
→ 기존 태스크를 새 설정으로 교체하려면 --force-new-deployment 사용
```

---

기존에는 UI 서비스가 로드 밸런서에만 연결되어 있었지만, 이 명령어를 실행하면 UI 서비스도 `retailstore.local` 네임스페이스 안에서 `ui`라는 이름으로 호출될 수 있게 됩니다. 또한 Service Connect 관련 로그와 접근 로그를 CloudWatch Logs에 남기도록 설정하고, 새 배포가 안정화될 때까지 기다립니다.

---

`"logConfiguration"`은 Service Connect 관련 로그를 어디로 보낼지 정하는 설정입니다. 여기서는 CloudWatch Logs로 보내도록 설정되어 있습니다.

---

`"accessLogConfiguration"`은 Service Connect의 접근 로그 설정입니다. 이 설정은 Service Connect를 통해 들어오고 나가는 요청 정보를 어떤 형식으로 남길지 정합니다.

`"format": "JSON"`은 접근 로그를 JSON 형식으로 남기겠다는 뜻입니다.

`"includeQueryParameters": "ENABLED"`는 요청 URL에 포함된 query parameter까지 접근 로그에 남기겠다는 설정입니다. 같은 API 경로라도 query parameter에 따라 요청 의미가 달라질 수 있기 때문에, 장애 분석이나 요청 추적을 더 정확하게 하기 위해 사용합니다. 다만 query parameter에 민감한 값이 포함될 수 있다면 로그에 남기는 것이 보안상 부담이 될 수 있습니다.

---

`"services"` 설정은 UI 서비스를 Service Connect에 `ui`라는 이름으로 등록하기 위한 설정입니다. 이를 통해 UI 서비스는 로드 밸런서로부터 외부 트래픽을 받는 역할뿐만 아니라, 다른 내부 서비스가 `http://ui` 또는 `ui:80`으로 호출할 수 있는 내부 서비스 이름도 가지게 됩니다.

---

요약하자면, 이 업데이트 명령어는 기존 UI 서비스에 Service Connect 설정을 추가합니다. 기존 UI 서비스는 로드 밸런서를 통해 외부 트래픽을 받는 구조였지만, 업데이트 이후에는 `retailstore.local` 네임스페이스 안에서 `ui`라는 이름으로도 등록됩니다. 따라서 다른 내부 서비스가 필요할 경우 `ui` 또는 `ui:80`으로 UI 서비스를 호출할 수 있습니다. 또한 Service Connect 관련 로그와 접근 로그를 CloudWatch Logs에 남기도록 설정하고, `--force-new-deployment` 옵션을 통해 기존 Task를 새 설정이 반영된 Task로 교체합니다.

1. UI 서비스에 Service Connect가 활성화됩니다.
2. UI 서비스가 `retailstore.local` 네임스페이스에 등록됩니다.
3. UI 서비스가 Service Connect 안에서 `ui`라는 이름을 갖게 됩니다.
4. 다른 내부 서비스가 `ui` 또는 `ui:80`으로 UI 서비스를 호출할 수 있게 됩니다.
5. Service Connect 통신 로그와 접근 로그가 CloudWatch Logs에 저장됩니다.
6. `--force-new-deployment`를 통해 기존 Task가 새 설정이 반영된 Task로 교체됩니다.

---
## Explorer Web Application

```bash
export RETAIL_ALB=$(aws elbv2 describe-load-balancers --name retail-store-ecs-ui \
 --query 'LoadBalancers[0].DNSName' --output text)

echo_c http://${RETAIL_ALB} ; echo
```

위 명령어는 UI 서비스에 접속할 로드 밸런서 주소를 가져와서 화면에 출력합니다.

>로드 밸런서의 DNS 주소는 외부 사용자가 애플리케이션에 접속할 때 사용하는 대표 주소입니다. 사용자가 이 주소로 접속하면 로드 밸런서가 요청을 받아 UI 서비스의 Task로 전달합니다. 따라서 이 구조에서는 로드 밸런서 주소가 애플리케이션의 외부 접속 주소 역할을 합니다.

위 명령어의 실행 결과입니다.

```bash
http://retail-store-ecs-ui-1660932406.ap-northeast-3.elb.amazonaws.com
```

Demo Store의 홈페이지 주소입니다.

---

Demo Store의 홈페이지 하단에있는 `Topology` 섹션을 클릭합니다.`http://retail-store-ecs-ui-1660932406.ap-northeast-3.elb.amazonaws.com/topology`로 이동합니다.

`http://retail-store-ecs-ui-1660932406.ap-northeast-3.elb.amazonaws.com/topology` 는 이 웹애플리케이션의 토폴로지를 보여줍니다. 여기서 토폴로지는 애플리케이션을 구성하는 마이크로서비스들이 어떻게 연결되어 있는지 보여주는 구조도를 의미합니다.

![[demo-store-topology.png]]

