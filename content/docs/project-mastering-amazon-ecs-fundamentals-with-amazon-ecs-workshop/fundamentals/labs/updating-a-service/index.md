---
title: "Updating a Services"
weight: 4
---
> **작성일:** 2026-05-09 | **수정일:** 2026-05-09

이번 섹션에서는 이전에 생성한 Task Definition을 수정해 새 리비전을 생성하여 이를 통해 UI 서비스의 테마를 변경합니다.

>리마인드 하자면, Task Definition이 ECS에 한번 등록된 후 내용이 수정될 때마다 새로운 리비전이 생성됩니다. 처음 등록된 리비전을 포함한 기존 리비전은 항상 남아있습니다.

---

기존 Task Definition의 내용에 다음과 같은 코드를 추가하여 새로운 Task Definition 파일을 생성합니다. 이는 애플리케이션 UI의 테마를 그린으로 바꿉니다.

```json
"environment": [
    {
        "name": "RETAIL_UI_THEME",
        "value": "green"
    }
]
```

다음 명령어는 위 코드가 반영된 Task Definition 파일을 생성합니다.

```bash
cat << EOF > retail-store-ecs-ui-updated-taskdef.json
```
```json
{
    "family": "retail-store-ecs-ui",
    "executionRoleArn": "arn:aws:iam::${ACCOUNT_ID}:role/retailStoreEcsTaskExecutionRole",
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
            "environment": [
                {
                    "name": "RETAIL_UI_THEME",
                    "value": "green"
                }
            ],
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
    ]
}
```
```bash
EOF
```

---

다음 명령어로 위 Task Definition 파일을 ECS에 등록합니다.

```bash
aws ecs register-task-definition --cli-input-json file://retail-store-ecs-ui-updated-taskdef.json
```

---

다음 명령어로 등록된 Task Definition의 리비전들을 볼 수 있습니다.

```bash
aws ecs list-task-definitions --family-prefix retail-store-ecs-ui --sort DESC --max-items 2
```

`--sort DESC --max-items 2` 옵션을 통해 최신 리비전 두 개를 보여줍니다.

다음은 이 명령어의 출력입니다.

```json
{
  "taskDefinitionArns": [
    "arn:aws:ecs:ap-northeast-3:802104112480:task-definition/retail-store-ecs-ui:2",
    "arn:aws:ecs:ap-northeast-3:802104112480:task-definition/retail-store-ecs-ui:1"
  ]
}
```

---

다음 명령어로 새로 등록된 Task Definition의 리비전을 사용하여 ECS 서비스를 업데이트합니다.

```bash
aws ecs update-service --cluster retail-store-ecs-cluster --service ui --task-definition retail-store-ecs-ui
```

---

위 ECS 서비스를 업데이트하는 명령어를 실행한 후 다음 명령어를 사용하여 ECS 서비스가 안정화될 때까지 기다립니다.

```bash
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

---

브라우저를 새로고침하면 변경된 애플리케이션의 UI를 확인할 수 있습니다.

![Demo Store New UI](retail-ui-service-new-theme.png)
