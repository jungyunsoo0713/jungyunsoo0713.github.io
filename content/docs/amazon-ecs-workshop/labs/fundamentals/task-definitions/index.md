---
title: "Task Definitions"
weight: 3
---
> **작성일:** 2026-04-22 | **수정일:** 2026-04-22
```json
cat << EOF > retail-store-ecs-ui-taskdef.json
{
    "family": "retail-store-ecs-ui",
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
            "image": "public.ecr.aws/aws-containers/retail-store-sample-ui:1.2.3",
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
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "retail-store-ecs-tasks",
                    "awslogs-region": "$AWS_REGION",
                    "awslogs-stream-prefix": "ui-service"
                }
            }
        }
    ],
    "executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskExecutionRole",
    "taskRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskRole"
}
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-ui-taskdef.json
```

ECS Fargate 태스크 정의(Task Definition)입니다. 

태스크 정의(Task Definition)란 ECS에서 태스크를 어떻게 실행할지 정의한 실행 설계도입니다. 태스크는 ECS에서 하나 이상의 컨테이너를 실행하는 단위이므로, 태스크 정의는 컨테이너 애플리케이션을 ECS에 띄우기 위해 필요한 설정들의 묶음이라고 볼 수 있습니다. 태스크 정의는 보통 이하의 내용을 가지고있습니다.

- 어떤 컨테이너 이미지를 쓸지
- CPU와 메모리를 얼마나 줄지
- 어떤 포트를 열지
- 환경 변수는 무엇인지
- 로그를 어디로 보낼지
- 어떤 IAM Role을 사용할지
- 헬스 체크를 어떻게 할지

---

`"family": "retail-store-ecs-ui"`는 이 태스크 정의가 속하는 `"family"` 값입니다. ECS에서는 같은 태스크 정의를 수정해 새 버전을 여러 번 등록할 수 있으며, 이 버전들은 같은 `"family"` 안에 묶입니다.

---

`"networkMode": "awsvpc"`는 이 태스크가 `awsvpc` 네트워크 모드로 실행된다는 의미입니다.

---

```json
    "requiresCompatibilities": [
        "FARGATE"
    ],
```

는 이 태스크 정의가 Fargate와 호환되는 방식으로 작성되었다는 의미입니다.

---

```json
    "cpu": "1024",
    "memory": "2048",
    "runtimePlatform": {
        "cpuArchitecture": "X86_64",
        "operatingSystemFamily": "LINUX"
    },
```

이 부분은 태스크의 실행 환경과 리소스 사양을 정의합니다. `"cpu": "1024"`는 1 vCPU를 사용한다는 뜻이고, `"memory": "2048"`는 2GB 메모리를 할당한다는 뜻입니다. `"runtimePlatform"`은 이 태스크가 어떤 플랫폼에서 실행될지를 지정합니다. `"cpuArchitecture": "X86_64"`는 x86_64 아키텍처의 CPU 환경에서 실행한다는 뜻이고, `"operatingSystemFamily": "LINUX"`는 Linux 운영체제 환경에서 실행한다는 뜻입니다.

---

```json
    "containerDefinitions": [
        {
            "name": "application",
            "image": "public.ecr.aws/aws-containers/retail-store-sample-ui:1.2.3",
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
```

`"containerDefinitions"`는 이 태스크에서 실행할 하나 이상의 컨테이너와 그 설정을 정의합니다.

---

```json
        {
            "name": "application",
            "image": "public.ecr.aws/aws-containers/retail-store-sample-ui:1.2.3",
```

`"name": "application"`은 컨테이너의 이름입니다. ECS는 태스크에 포함된 각 컨테이너를 이름으로 구분합니다.

`"image": "public.ecr.aws/aws-containers/retail-store-sample-ui:1.2.3"`는 이 컨테이너를 실행할 때 사용할 컨테이너 이미지를 의미합니다. 여기서는 Amazon ECR Public에 올라와 있는 `retail-store-sample-ui` 이미지의 `1.2.3` 버전을 사용합니다.

합쳐서 말하면, 이 설정은 `"application"`이라는 이름의 컨테이너를 `"public.ecr.aws/aws-containers/retail-store-sample-ui:1.2.3"` 이미지를 사용해서 실행한다는 뜻입니다.

---

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

`"portMappings"`는 컨테이너(`"application"`)의 포트를 설정합니다.

---

`"containerPort": 8080`는 컨테이너가 `8080`번 포트를 통해서 요청을 받는다는 의미입니다.

---

`"hostPort": 8080`는 호스트 측에서도 `8080`번 포트를 사용한다는 의미입니다.

여기서 host는 컨테이너 바깥을 감싸고 있는 실행 환경을 의미합니다.

---

`"protocol": "tcp"`는 네트워크가 통신할 때 `TCP` 프로토콜을 사용한다는 뜻이고, `"appProtocol": "http"`는 그 `TCP` 프로토콜 위에서 동작하는 `HTTP` 프로토콜을 이용해 컨테이너가 요청과 응답을 주고받는다는 뜻입니다.

---

```json
            "essential": true,
            "linuxParameters": {
                "initProcessEnabled": true
            },
```

`"essential": true`는 이 컨테이너가 필수 컨테이너라는 뜻으로, ECS 태스크 전체는 이 컨테이너가 정상 작동하지 않으면 실패한다고 간주합니다.

---

`"linuxParameters": { "initProcessEnabled": true }`는 이 컨테이너에서 `init` 프로세스를 활성화한다는 의미입니다. `init` 프로세스는 컨테이너 실행 시 가장 먼저 실행되는 프로세스로, 기본적인 프로세스 관리를 도와줍니다.

---

```json
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
```

컨테이너의 헬스 체크 설정입니다. `"command"`는 헬스 체크에 사용할 명령어를 의미합니다. 

---

`"CMD-SHELL"`은 이 명령(`"curl -f http://localhost:8080/actuator/health || exit 1"`)을 커맨드 셸을 통해 실행하겠다는 뜻입니다. 

---

`"curl -f http://localhost:8080/actuator/health || exit 1"` 명령어는 `http://localhost:8080/actuator/health`에 요청을 보냅니다. 이 주소는 컨테이너 안에서 자기 자신에게 보내는 헬스 체크용 주소입니다. 즉, ALB가 보내는 헬스 체크가 아니라 컨테이너가 자기 자신의 상태를 점검하기 위한 요청입니다.

`curl -f`는 `HTTP` 요청이 실패하면 에러로 처리하고, `|| exit 1`은 요청이 실패하면 종료 코드 `1`을 반환합니다.

---

`"interval"`, `"timeout"`, `"retries"`, `"startPeriod"`는 각각 헬스 체크를 수행하는 간격, 헬스 체크 요청 후 응답을 기다리는 시간, 연속 실패 시 비정상으로 판단하는 기준 횟수, 그리고 컨테이너 시작 후 초기 준비 시간으로 간주하는 유예 기간을 의미합니다. 이 기간 동안의 헬스 체크 실패는 바로 재시도 횟수에 반영되지 않습니다. 간단히 말하면, `"startPeriod"` 기간 동안에는 헬스 체크가 성공하면 정상 성공으로 간주되고, 실패하더라도 바로 `"retries"`에 반영되지 않습니다. 단, 이 기간 안에 한 번이라도 성공한 뒤에는 이후 실패가 정상적으로 카운트됩니다.

---

```json
            "versionConsistency": "disabled",
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "retail-store-ecs-tasks",
                    "awslogs-region": "$AWS_REGION",
                    "awslogs-stream-prefix": "ui-service"
                }
            }
```

`"versionConsistency": "disabled"`는 ECS가 이 컨테이너 이미지 태그를 `digest`로 고정하지 않고, 태스크 정의에 적힌 원래 이미지 URI를 그대로 사용하도록 하는 설정입니다. 여기서 `digest`는 컨테이너 이미지의 내용 자체를 가리키는 고정된 식별자입니다. `digest`로 고정하지 않으면 이미지 태그를 그대로 사용하게 되며, 이 경우 같은 태그가 나중에 다른 이미지 내용을 가리키게 될 수도 있습니다.

예시를 들면, `public.ecr.aws/aws-containers/retail-store-sample-ui:latest`에서 `latest`는 이 이미지의 태그입니다. 이 태그는 사람이 알아보기 쉬운 이름표이지만, 나중에 같은 태그가 다른 이미지 내용을 가리키게 바뀔 수도 있습니다. 반면 `digest`로 고정하면 `public.ecr.aws/aws-containers/retail-store-sample-ui@sha256:...`처럼 이미지 내용 자체를 기준으로 한 고정 식별자를 사용하게 되므로, 항상 동일한 이미지를 가리키게 됩니다. 따라서 나중에 다른 이미지가 이 컨테이너에 사용될 가능성을 줄일 수 있습니다. 태그를 사용하는 이유는 이 고정 식별자만으로는 어떤 이미지를 정확히 의미하는지 한눈에 알아보기 어렵기 때문입니다. 태그를 사용하면 해당 이미지가 무엇인지 더 쉽게 파악할 수 있습니다

---

`"logConfiguration"`은 컨테이너의 로그를 어떤 식으로 다룰지 정의하는 설정입니다. 여기에는 로그를 어떻게 수집하고 어디로 보낼지가 포함됩니다.

---

`"logDriver"`는 어떤 로그 드라이버를 사용할 지에 대한 옵션입니다. 여기서는 `"awslogs"`를 사용합니다. `"awslogs"`는 컨테이너의 로그를 Amazon CloudWatch Logs로 보내는 로그 드라이버입니다.

참고로 Fargate에서 사용할 수 있는 로그 드라이버로는 `awslogs`, `splunk`, `awsfirelens`가 있습니다. 여기서는 그중 `awslogs`를 사용해 컨테이너 로그를 CloudWatch Logs로 보냅니다.

---

`"options"`로 로그에 관한 세부 설정을 합니다. `"awslogs-group"`은 로그 그룹 이름, `"awslogs-region"`은 로그가 전달될 리전, `"awslogs-stream-prefix"`는 로그 스트림 이름 앞에 붙일 접두사를 정합니다.

참고로 로그 그룹은 관련된 로그를 모아두는 집합이고, 로그 스트림은 같은 소스에서 나온 로그들이 시간 순서대로 쌓이는 한 줄기라고 볼 수 있습니다. 위의 코드에서 `retail-store-ecs-tasks`가 로그 그룹이라면, 그 안의 `ui-service/application/ecs-task-1`은 로그 스트림입니다. 즉 로그 그룹은 폴더이고, 로그 스트림은 그 안의 개별 로그 묶음이라고 보면 됩니다.

---

```json
    "executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskExecutionRole",
    "taskRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskRole"
```

IAM Role 리소스 식별자(ARN)을 의미합니다.

`"executionRoleArn"`은 ECS가 태스크를 실행하는 과정에서 사용하는 IAM Role의 리소스 식별자입니다. ECR에서 컨테이너 이미지를 가져오거나 CloudWatch Logs로 로그를 전송하는 작업에 사용됩니다.

---

`"taskRoleArn"`은 컨테이너 내부 애플리케이션이 사용하는 IAM Role의 리소스 식별자입니다. 이 코드에서 Role에 연결된 정책을 보면, 컨테이너는 `ssmmessages` 권한을 통해 ECS Exec에 필요한 세션 채널을 생성하고 열 수 있으며, `elasticfilesystem` 권한을 통해 지정된 EFS 파일 시스템을 마운트하고 쓰기 작업을 수행할 수 있습니다.