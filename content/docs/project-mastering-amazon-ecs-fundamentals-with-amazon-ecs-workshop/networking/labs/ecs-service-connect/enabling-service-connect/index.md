---
title: "Enabling Service Connect"
weight: 1
---
> **작성일:** 2026-05-12 | **수정일:** 2026-05-12

이번 섹션에서는 UI 서비스가 통신할 세 개의 마이크로서비스를 배포합니다. 또한 이 서비스들에 ECS Service Connect 기능을 활성화하여 클러스터에서 ECS Service Connect를 사용하도록 설정합니다.

---

## Orders 서비스 배포

다음 명령어로 Task Definition 파일을 생성하고 ECS에 등록합니다.

```bash
cat << EOF > retail-store-ecs-order-taskdef.json
```
```json
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
```
```bash
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-order-taskdef.json
```

`"executionRoleArn": arn:aws:iam::${ACCOUNT_ID}:role/ordersEcsTaskExecutionRole"`는. 현재 AWS 계정의 `ordersEcsTaskExecutionRole` IAM Role을 사용합니다. 각 ECS 서비스마다 사용하는 Task Execution Role이 전부 다릅니다. UI 서비스 경우에는 `retailStoreEcsTaskRole` IAM Role을 사용했습니다.

---

`"cpu"`는 태스크에 할당된 CPU 지분 중 이 컨테이너에 할당할 지분을 의미합니다. 여기서는 값이 `0`이므로 태스크에 할당된 CPU 범위 안에서 실행됩니다. 이 설정은 기본값이므로 굳이 명시해주지 않아도 됩니다.

---

`"environment"`는 이 컨테이너의 환경 변수 값을 설정합니다.

`"RETAIL_ORDERS_PERSISTENCE_PROVIDER"`는 Orders 서비스가 어떤 저장소를 사용할지 지정하는 환경 변수입니다. 변수값은 `"postgres"`입니다. 이 환경 변수가 필요한 이유는 Orders 서비스가 여러 종류의 저장소를 지원할 수 있기 때문입니다.

`"RETAIL_ORDERS_MESSAGING_PROVIDER"`는 Orders 서비스가 어떤 메시징 방식을 사용할지 지정하는 환경 변수입니다. 변수값은  `"in-memory"`입니다. 이는 Orders 서비스에서 발생하는 메시지를 다른 외부 메시징 툴을 사용하지 않고 자체 메모리 안에서 처리한다는 것입니다. 즉 자신의 내부 로직에서 이 발생한 메시지를 사용합니다. 

여기서 메시지란 이벤트 데이터를 의미합니다. 예를 들어, 주문 생성 이벤트가 발생하면 다음과 같은 메시지가 만들어집니다.

```json
{
  "eventType": "OrderCreated",
  "orderId": "order-123",
  "customerId": "user-456",
  "items": [
    {
      "productId": "product-1",
      "quantity": 2
    }
  ]
}
```

마이크로서비스에서 발생한 메시지는 외부의 메시징 툴이 처리할 수 있습니다. 실무에서는 마이크로서비스끼리 직접 호출하지 않고 Amazon SQS, SNS, Kafka, Kafka Streams 등의 툴이 발생한 메시지를 대신 전달하거나 처리할 수 있습니다.

위에 언급된 메시징 툴 중에서, Amazon SQS(Simple Queue Service)는 발생한 메시지를 받아서 잠시 자신의 큐에 대기시켜 놓는 대기열 서비스입니다. 

SQS가 필요한 이유는 메시지를 비동기적으로 처리하게 해주기 때문입니다. 기본적으로 API 요청은, 요청에 대한 응답을 받기 전까지는 해당 처리 흐름이 대기하게 됩니다. 응답 대기 시간이 길어지면 전체적인 작업 속도의 저하를 가져옵니다. 

SQS는 마이크로서비스가 보낸 요청 메시지를 대신 받아서 자신의 큐에 대기시켜 놓고, 메시지를 받을 주체가 나중에 이 메시지를 가져갑니다. 이 과정에서 메시지를 보낸 주체는 즉각적으로 응답을 받을 수 있기 때문에 요청 처리 흐름이 대기하는 시간을 최소화할 수 있습니다. 또한 메시지를 받는 주체는 자신의 리소스에 여유가 있을 때 메시지를 가져올 수 있기 때문에, 트래픽 증가를 버티거나 장애 시 메시지 유실을 줄일 수 있습니다.

SQS는 마이크로서비스가 보낸 응답 메시지도 대신 받아서 자신의 큐에 대기시켜 놓고, 메시지를 받을 주체가 나중에 이 메시지를 가져갑니다. 응답 메시지를 받는 주체는 마찬가지로 자신의 리소스에 여유가 있을 때 메시지를 가져올 수 있기 때문에, 같은 원리로 트래픽 증가를 버티거나 장애 시 메시지 유실을 줄일 수 있습니다.

>SQS는 메시지를 받으면, 메시지를 보낸 주체에게 "메시지를 정상적으로 저장했다"라는 성공 응답을 줍니다. 이는 요청/응답 메시지 모두에 해당됩니다.

>SQS가 요청 메시지를 자신의 큐에 대기시키는 것은 기본 흐름이지만, 응답 메시지를 자신의 큐에 대기시켜 놓는 것은 옵션에 가깝습니다.

---

`"secrets"`은 이 컨테이너의 기밀 값을 설정합니다.

`"RETAIL_ORDERS_PERSISTENCE_ENDPOINT"`는 Orders 서비스의 저장소 엔드포인트 값을 의미합니다. 값의 출처는 `"arn:aws:ssm:${AWS_REGION}:${ACCOUNT_ID}:parameter/retail-store-ecs/orders/db-endpoint-postgres"`이며, Systems Manager Parameter Store에 있는 DB 엔드포인트 파라미터의 ARN을 참조합니다.

`"RETAIL_ORDERS_PERSISTENCE_NAME"`은 Orders 서비스의 저장소 이름을 의미합니다. 값의 출처는 `"arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:dbname::"`이며, Secrets Manager에 있는 `retail-store-ecs-orders-db` 시크릿의 `dbname` 값을 참조합니다.

`"RETAIL_ORDERS_PERSISTENCE_USERNAME"`은 Orders 서비스의 저장소 사용자 이름을 의미합니다. 값의 출처는 `"arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:username::"`이며, Secrets Manager에 있는 `retail-store-ecs-orders-db` 시크릿의 `username` 값을 참조합니다.

`"RETAIL_ORDERS_PERSISTENCE_PASSWORD"`은 Orders 서비스의 저장소 사용자 비밀번호를 의미합니다. 값의 출처는 `"arn:aws:secretsmanager:${AWS_REGION}:${ACCOUNT_ID}:secret:retail-store-ecs-orders-db:password::"`이며, Secrets Manager에 있는 `retail-store-ecs-orders-db` 시크릿의 `password` 값을 참조합니다.

> DB 엔드포인트만 Parameter Store에서 관리되는 이유는 상대적으로 DB 이름, 사용자 이름, 사용자 비밀번호보다 덜 민감한 정보이기 때문입니다.

---

`"awslogs-stream-prefix": "orders-service"`를 보면 UI 서비스가 사용하는 로그 그룹과 같은 로그 그룹에 속해 있지만, 다른 로그 스트림의 프리픽스를 사용함을 알 수 있습니다.

---

다음 명령어로 Orders 서비스를 ECS 클러스터에 생성합니다.

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

`--desired-count 1`를 보면 UI 서비스를 생성하는 명령어의 옵션 값인 `2`와 다르게 `1`로 설정된 것을 알 수 있습니다. 이는 UI 서비스가 받는 트래픽이 더 많기 때문에 유지해야 할 태스크가 더 많아야 됨을 추측할 수 있습니다. UI 서비스는 ALB가 받는 클라이언트의 요청을 모두 받는 서비스이기 때문에, UI 서비스로부터 분산된 요청을 받는 Orders 서비스보다 처리량이 더 많을 수 있습니다.

---

`--service-connect-configuration`은 ECS 서비스에 ECS Service Connect 기능을 활성화합니다.

`"enabled": true`는 이 기능을 활성화한다는 의미입니다.

`"namespace"`은 ECS Service Connect가 사용할 네임스페이스 이름을 의미합니다. 이전에 생성한 Private Namespace인 `retailstore.local`을 사용합니다.

---

`"services"`는 ECS Service Connect에 등록할 이 서비스에 대한 설정을 의미합니다.

`"portName"`은 ECS Service Connect 기능을 통해 이 태스크에 요청이 왔을 때 요청을 전달할 포트 매핑의 이름입니다. 여기서 이 이름은 `"application"`입니다.

다음은 이전에 작성한 Task Definition의 `"portMappings"`입니다.

```json
            "portMappings": [
                {
                    "name": "application",
                    "containerPort": 8080,
                    "hostPort": 8080,
                    "protocol": "tcp",
                    "appProtocol": "http"
                }
            ],
```

`"discoveryName"`은 ECS Service Connect가 이 ECS 서비스를 Cloud Map에 등록할 때 사용할 이름입니다. ECS 서비스 태스크에서 실행 중인 애플리케이션이 DNS 이름인 `orders`와 포트 번호 80을 사용해서 요청을 하면, Cloud Map에 등록된 Orders 서비스 정보를 사용해서 Orders 서비스 태스크의 IP/포트로 요청을 전달합니다.

`"clientAliases"`는 이 ECS 서비스가 어떤 DNS 이름과 포트 번호로 ECS Service Connect 기능에 의해 접근되는지를 의미합니다. 여기서 포트 번호는 `80`이고 DNS 이름은 `"orders"`입니다.

`"discoveryName"`과 `"clientAliases"`의 `"dnsName"`은 반드시 같지 않아도 됩니다. 하지만 대부분 편의상 이 둘을 같게 설정합니다.

---

## Checkout 서비스 배포

다음 명령어로 Task Definition 파일을 생성하고 ECS에 등록합니다.

```bash
cat << EOF > retail-store-ecs-checkout-taskdef.json
```
```json
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
```
```bash
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-checkout-taskdef.json
```

`"environment"`를 보면 Orders 서비스의 Task Definition과 다르게, Messaging Provider를 사용하지 않았음을 알 수 있습니다. 

`"RETAIL_CHECKOUT_ENDPOINTS_ORDERS"`는 Checkout 서비스가 Orders 서비스에게 요청을 보낼 때 사용할 API 엔드포인트를 의미합니다.

---

`"RETAIL_CHECKOUT_PERSISTENCE_REDIS_URL"`는 Checkout 서비스의 저장소 엔드포인트 값을 의미합니다. 값의 출처는 `"arn:aws:ssm:${AWS_REGION}:${ACCOUNT_ID}:parameter/retail-store-ecs/checkout/redis-endpoint"`이며, Systems Manager Parameter Store에 있는 Redis 엔드포인트 파라미터의 ARN을 참조합니다.

Orders 서비스의 Task Definition과 다르게 사용자 이름과 사용자 비밀번호는 `secrets`에 존재하지 않습니다. 이는 Redis로 접근할 때는 사용자 이름과 사용자 비밀번호가 필요 없기 때문입니다.

---

`"command"`의 `"curl -f http://localhost:8080/health || exit 1"`를 보면 Orders 서비스의 값인 `"curl -f http://localhost:8080/actuator/health || exit 1"`과 다름을 알 수 있습니다. Orders 서비스는 스프링 부트 기반이기 때문에 Spring Boot Actuator에서 제공하는 기본 헬스 체크 엔드포인트로 `http://localhost:8080/actuator/health`를 사용하기 때문입니다. 

---

다음 명령어로 Checkout 서비스를 ECS 클러스터에 생성합니다.

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

---

## Catalog 서비스 배포

다음 명령어로 Task Definition 파일을 생성하고 ECS에 등록합니다.

```bash
cat << EOF > retail-store-ecs-catalog-taskdef.json
```
```json
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
```
```bash
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-catalog-taskdef.json
```

Catalog 서비스는 Orders 서비스와는 다르게 DB 이름은 `"environment"`에 지정합니다. DB 이름은 보통 DB 비밀번호처럼 민감한 정보가 아니기 때문에 환경 변수에 있어도 괜찮습니다.

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

Catalog 서비스를 생성하는 명령어의 `--desired-count` 값은 `2`로, Orders 서비스를 생성하는 명령어와 Checkout 서비스를 생성하는 명령어의 값인 `1`보다 큼을 알 수 있습니다. 이는 쇼핑 애플리케이션 이용 시 상품을 고르는 데 더 많은 시간이 쓰이기 때문임을 짐작할 수 있습니다.

---

## UI 서비스 업데이트하기 

위 ECS 서비스들을 업데이트하는 명령어를 실행한 후 다음 명령어를 사용하여 ECS 서비스가 안정화될 때까지 기다립니다.

```
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services orders
aws ecs wait services-stable --cluster retail-store-ecs-cluster --services checkout
aws ecs wait services-stable --cluster retail-store-ecs-cluster --services catalog
```

다음 명령어를 실행하여 `retail-store-ecs-ui-connect-taskdef.json`을 생성하고 UI 서비스의 Task Definition을 출력합니다.

```bash
lab-prep connect | jq
```

새로운 UI 서비스의 Task Definition을 등록합니다. 

다음 명령어를 실행하여 UI 서비스 태스크를 업데이트 합니다. 

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
```

위 ECS 서비스를 업데이트하는 명령어를 실행한 후 다음 명령어를 사용하여 ECS 서비스가 안정화될 때까지 기다립니다.

```bash
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

---
## 웹 애플리케이션 둘러보기

다음 명령어를 실행하여 현재 실행 중인 UI 서비스에서 사용 중인 로드 밸런서의 URL을 가져옵니다.

```bash
export RETAIL_ALB=$(aws elbv2 describe-load-balancers --name retail-store-ecs-ui \
 --query 'LoadBalancers[0].DNSName' --output text)

echo_c http://${RETAIL_ALB} ; echo
```

이 명령어의 출력입니다.

```bash
http://retail-store-ecs-ui-1878288695.ap-northeast-3.elb.amazonaws.com
```

이번 섹션에서는 기존 UI 서비스에 더해, Orders 서비스, Checkout 서비스, Catalog 서비스를 추가했습니다. 각 서비스는 ECS Service Connect 기능을 사용할 수 있습니다


