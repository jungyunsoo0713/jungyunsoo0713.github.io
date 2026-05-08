---
title: "Task Definitions"
weight: 2
---
> **작성일:** 2026-05-08 | **수정일:** 2026-05-08

다음은 ECS 클러스터에 제일 먼저 생성할 UI 서비스에서 실행할 태스크의 Task Definition입니다.

```json
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
```

`"family": "retail-store-ecs-ui"`는 이 Task Definition이 속할 패밀리 그룹을 의미합니다. 만약 이 Task Definition이 수정되어 새로 등록되면 같은 패밀리 그룹 내에서 새 revision이 생성됩니다.

다음은 revision의 예시입니다.

```
retail-store-ecs-ui:1
retail-store-ecs-ui:2
retail-store-ecs-ui:3
```

여기서 family는 `retail-store-ecs-ui`이고, `:` 뒤의 숫자들이 각 revision을 의미합니다.

---

`"networkMode": "awsvpc"`는 이 Task Definition으로부터 생성된 태스크가 사용할 네트워크 모드입니다. 이 `"awsvpc"` 네트워크 모드는 각 태스크에 별도의 ENI를 할당하며, 이 ENI가 가지고 있는 IP 주소의 값은 각 태스크마다 다릅니다.

---

`"requiresCompatibilities"`는 이 Task Definition으로부터 생성된 태스크의 Launch Type을 지정합니다. 즉, 이 태스크를 어디에서 실행할 수 있는지(예: EC2, FARGATE, EXTERNAL 등의 태스크 실행 환경)를 지정합니다. 

주의할 점으로는 이 필드는 태스크가 사용할 수 있는 실행 환경을 지정하는 것일 뿐, 실제로 해당 실행 환경에서 반드시 실행된다고 보장하는 것은 아닙니다. 실제로 해당 실행 환경에서 실행되도록 하는 설정은 ECS 서비스를 생성할 때 사용하는 명령어인 `aws ecs create-service`의 옵션인 `--capacity-provider-strategy` 혹은 `--launch-type`에서 지정하거나, ECS 서비스를 만들지 않고 태스크를 직접 한 번 실행하는 명령어인 `aws ecs run-task`의 옵션인 `--launch-type`에서 지정할 수 있습니다.

---

`"cpu"`와 `"memory"`는 이 태스크에 할당할 CPU와 메모리 사양을 지정합니다. `"cpu": "1024"`에서 `1024`는 1 vCPU를 의미합니다. vCPU란 AWS에서 한 개의 가상 CPU를 의미합니다. 즉, CPU 컴퓨팅 자원의 단위입니다. 

>vCPU는 지정된 하드웨어의 CPU 컴퓨팅 단위이므로, 하드웨어가 다를 시 CPU 컴퓨팅 성능도 다를 수 있습니다. Fargate는 AWS가 완전 관리하기 때문에 이 개념은 EC2 Launch Type에서 더 직접적으로 적용됩니다.

---

`"runtimePlatform"`은 이 태스크가 실행되는 환경의 플랫폼을 지정합니다. 즉, 이 태스크가 실행되는 서버의 CPU 아키텍처와 운영체제를 지정합니다.

---

`"containerDefinitions"`는 이 태스크 안에서 실행할 컨테이너들을 정의합니다.

`"name": "application"`는 이 컨테이너의 이름입니다. 주의할 점으로 이는 태스크의 이름이 아닌 컨테이너의 이름이라는 것입니다. 태스크에는 따로 이름이 없고 식별자가 존재합니다.

`"image": "public.ecr.aws/aws-containers/retail-store-sample-ui:1.2.3"`는 Amazon ECR Public에 있는 컨테이너 이미지입니다. ECR은 Elastic Container Registry의 약자로, 컨테이너 이미지를 가지고 있는 저장소입니다. VPC 내부에 존재하는 저장소가 아니라, AWS에 존재하는 저장소입니다.

`"portMappings"`는 컨테이너의 포트를 설정합니다.

`"name": "application"`은 포트를 설정할 컨테이너를 지정합니다.

`"containerPort": 8080`은 컨테이너의 포트 번호를 지정합니다.

`"hostPort": 8080`는 이 컨테이너를 실행하고 있는 호스트의 포트 번호를 지정합니다. `"awsvpc"` 네트워크 모드의 경우에는, 호스트의 포트 번호는 이 컨테이너를 실행하고 있는 태스크의 ENI의 포트 번호를 의미합니다. EC2 기반 실행 환경의 경우에는 호스트는 EC2 컨테이너 인스턴스를 가리키고, Fargate 실행 환경의 경우에는 사용자가 직접 관리하는 호스트가 존재하지 않습니다. Fargate는 AWS가 완전 관리하는 서버리스 실행 환경이기 때문입니다.

`"protocol": "tcp"`는 이 컨테이너와 통신하는 데 사용되는 트래픽의 프로토콜이 `TCP`라는 것을 의미합니다.

`"appProtocol": "http"`는 이 컨테이너에서 실행되는 애플리케이션과 통신하는 데 사용되는 트래픽의 프로토콜이 `HTTP`라는 것을 의미합니다. 주의할 점으로는 파라미터의 값 `HTTP`는 `HTTPS`를 포함하지 않은 `HTTP` 프로토콜만을 의미합니다. `"awsvpc"` 네트워크 모드에서는 `"containerPort"`와 `"hostPort"`의 값이 동일해야 합니다. 때문에 컨테이너가 받은 트래픽을 그대로 애플리케이션에 전달할 수 있게 됩니다.

---

`"essential": true`는 이 컨테이너가 필수 컨테이너라는 것을 의미합니다. ECS는 이 컨테이너가 정상 작동하지 않는다면, 이 컨테이너를 실행시키는 태스크를 비정상적인 태스크로 간주합니다. 비정상적인 태스크로 간주된다면, ECS는 이 태스크를 중지하고 새 태스크를 생성해서 교체하려고 합니다. 이 중지된 태스크들은 추후에 ECS가 자동으로 정리합니다.

---

`"linuxParameters"`는 컨테이너의 Linux 관련 설정을 하는 필드입니다.

`"initProcessEnabled": true`는 컨테이너 안에서 init 프로세스를 활성화시키는 설정입니다. init 프로세스는 컨테이너에서 가장 먼저 실행되는 프로세스로, 다른 핵심 프로세스들을 시작시키고, 좀비 프로세스나 남아 있던 자식 프로세스들을 정리하는 등의 역할을 합니다. 이 필드의 값이 `false`라면 애플리케이션 프로세스가 첫 번째 프로세스가 되어, init 프로세스가 하는 역할을 수행하지 못할 수 있습니다.

자식 프로세스란 프로세스가 만들어낸 하위 프로세스입니다. 애플리케이션이 종료되거나 재시작될 때, 기존 애플리케이션 프로세스가 만든 자식 프로세스가 종료되지 않은 채 남아 있다면, 좀비 프로세스나 불필요한 리소스 점유 등이 일어날 수 있습니다.

---

`"healthCheck"`는 태스크에서 실행하는 컨테이너의 헬스 체크에 대한 설정입니다. 이 요청을 보내는 주체는 태스크나 서비스가 아닌 ECS입니다.

`"command"`는 헬스 체크에 사용될 명령어를 지정합니다. 

`"CMD-SHELL"`는 이 명령어를 컨테이너가 사용하는 기본 셸을 사용하겠다는 의미입니다. 따라서 대부분 `/bin/sh` 같은 기본셸을 사용합니다. `/bin/sh` 셸은 기본 POSIX 셸에 가깝습니다. 

>각 컨테이너는 자신만의 Linux 사용자 공간을 가지고 있고, 호스트의 커널을 공유해서 사용하는 구조입니다. 컨테이너 이미지 안의 Linux 사용자 공간에 설치되어 있는 셸을 통해 명령어를 사용할 수 있습니다.

>Linux 운영체제는 Linux 커널(Kernel Space)과 Linux 사용자 공간(User Space)을 합친 것을 의미합니다. 엄밀히 말하자면 Linux는 리눅스의 커널만을 지칭하는 것이지만, 일반적으로 Linux는 Linux 운영체제를 뜻합니다.

`"curl -f http://localhost:8080/actuator/health || exit 1"`는 헬스 체크에 사용될 명령어 본문을 의미합니다. `http://localhost:8080/actuator/health` 이 주소로 헬스 체크 요청을 보내고, 만약 요청에 실패한다면(응답을 받지 못하거나, 정상 응답을 받지 못하는 경우) 종료 코드 1을 반환하라는 의미입니다.

`"interval": 10`은 10초마다 자동으로 헬스 체크 요청을 보낸다는 의미입니다.

`"timeout": 5`는 헬스 체크 요청 후 최대 5초까지 기다리겠다는 의미입니다. 5초 안에 정상 응답을 받지 못하면, 해당 헬스 체크는 실패로 간주합니다.

`"retries": 3`는 헬스 체크 실패 후 최대 3번까지 다시 헬스 체크를 시도하겠다는 의미입니다. 3번 연속 실패 시 해당 컨테이너를 비정상으로 간주합니다.

`"startPeriod": 30`는 컨테이너가 실행된 후 30초 동안 헬스 체크에 실패해도 실패로 간주하지 않겠다는 의미입니다. 컨테이너가 실행된 후 정상 작동을 위한 시간이 필요하기 때문입니다.

---

`"versionConsistency": "disabled"`는 ECS가 컨테이너 이미지를 참조할 때, 컨테이너 이미지의 고유 식별자(digest)를 참조하는 digest 방식을 사용하지 않고, 컨테이너 이미지의 Tag를 참조하는 방식을 의미합니다. 컨테이너 이미지의 Tag를 참조하는 방식은 참조하는 컨테이너 이미지가 무엇인지 직관적으로 알게 해줍니다. 하지만 동일한 Tag가 나중에 다른 컨테이너 이미지에 다시 붙을 수 있기 때문에, 실수로 다른 컨테이너 이미지를 참조할 수 있게 됩니다.

---

`"logDriver": "awslogs"`는 이 컨테이너의 로그를 CloudWatch로 전송하겠다는 의미입니다.
`awslogs`말고도 `splunk`나 `awsfirelens` 등의 값이 올 수 있습니다. 필드 값 `splunk`는 컨테이너의 로그를 Splunk로 전송하겠다는 의미를 가집니다. Splunk는 로그와 이벤트를 분석하는 툴로, AWS 서비스가 아닌 써드파티 서비스입니다. `awsfirelens`의 경우에는 컨테이너의 로그를 FireLens를 통해 다른 여러 로그 및 이벤트 분석 서비스에 전달합니다.

---

`"awslogs-group": "retail-store-ecs-tasks"`는 컨테이너 로그를 전송할 로그 그룹의 이름을 지정합니다.

`"awslogs-region": "$AWS_REGION"`는 컨테이너의 로그 그룹이 속할 리전을 지정합니다. `$AWS_REGION`은 현재 컨테이너가 실행되는 리전을 동적으로 가져옵니다.

`"awslogs-stream-prefix": "ui-service"`는 컨테이너의 로그 그룹에 저장되는 로그 스트림의 prefix를 지정합니다.

---

`"executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskExecutionRole"`는 이 컨테이너를 실행하기 위해 필요한 IAM Role을 지정합니다. 현재 AWS 계정의 `retailStoreEcsTaskExecutionRole` IAM Role을 사용합니다.

`"taskRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskRole"`는 이 컨테이너가 실행 중에 사용할 IAM Role을 지정합니다. 현재 AWS 계정의 `retailStoreEcsTaskRole` IAM Role을 사용합니다.

---

```bash
cat << EOF > retail-store-ecs-ui-taskdef.json
{
"family": "retail-store-ecs-ui",
"networkMode": "awsvpc",
...
	"executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskExecutionRole",
	"taskRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskRole"
}
EOF

aws ecs register-task-definition --cli-input-json file://retail-store-ecs-ui-taskdef.json
```

앞서 작성한 Task Definition을 포함한 이 JSON 파일을 생성한 뒤, Task Definition을 ECS에 등록하는 명령어를 사용하여 Task Definition을 등록합니다.

---

다음 명령어는 등록한 Task Definition을 가져오는 명령어입니다.

```bash
aws ecs describe-task-definition --task-definition retail-store-ecs-ui
```
