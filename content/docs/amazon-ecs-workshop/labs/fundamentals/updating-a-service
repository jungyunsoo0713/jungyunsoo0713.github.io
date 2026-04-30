---
title: "Updating a Services"
weight: 4
draft: true
---
> **작성일:** 2026-04-30 | **수정일:** 2026-04-30

이번 섹션에서는 ECS Service를 업데이트해 애플리케이션 설정을 변경하는 과정을 배웁니다. 여기서는 UI 서비스에 `RETAIL_UI_THEME`이라는 환경 변수를 추가해 화면의 기본 색상 테마를 바꿉니다. Docker 이미지는 직접 수정하지 않습니다. 대신 Task Definition을 수정해 컨테이너가 실행될 때 환경 변수를 전달하고, 애플리케이션은 이 값을 읽어 동작을 변경합니다.

다음은 Spring Boot 애플리케이션에서 환경 변수를 읽는 예제 코드입니다.

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ThemeController {

    @Value("${RETAIL_UI_THEME:default}")
    private String theme;

    @GetMapping("/theme")
    public String getTheme() {
        return theme;
    }
}
```

`@Value("${RETAIL_UI_THEME:default}")`는 `RETAIL_UI_THEME` 환경 변수 값을 읽어 `theme` 변수에 넣습니다. 만약 해당 환경 변수가 없다면 기본값으로 `default`를 사용합니다.

ECS Task Definition에서는 다음과 같이 컨테이너에 환경 변수를 전달할 수 있습니다.

```json
...
            "environment": [
                {
                    "name": "RETAIL_UI_THEME",
                    "value": "green"
                }
            ],
...
```

이렇게 설정하면 컨테이너 안에서 실행되는 Spring Boot 애플리케이션은 `RETAIL_UI_THEME=green` 값을 읽을 수 있고, 애플리케이션은 이 값을 기준으로 화면 테마를 변경할 수 있습니다.

---

Task Definition을 수정합니다.

```json
cat << EOF > retail-store-ecs-ui-updated-taskdef.json
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
EOF
```

---

그 후 `aws ecs register-task-definition --cli-input-json file://retail-store-ecs-ui-updated-taskdef.json` 이 명령어로 Task Definition을 업데이트 합니다.

---

```bash
aws ecs list-task-definitions --family-prefix retail-store-ecs-ui --sort DESC --max-items 2
```

위 명령어는 `retail-store-ecs-ui`로 시작하는 Task Definition들을 최신순으로 조회합니다. Task Definition을 수정해 다시 등록하면 기존 revision이 수정되는 것이 아니라 새로운 revision이 생성되므로, 이 명령어를 통해 이전 revision과 새 revision을 함께 확인할 수 있습니다.

위 명령어의 출력 입니다.

```json
{
  "taskDefinitionArns": [
    "arn:aws:ecs:ap-northeast-3:802104112480:task-definition/retail-store-ecs-ui:4",
    "arn:aws:ecs:ap-northeast-3:802104112480:task-definition/retail-store-ecs-ui:3"
  ]
}
```

---

```bash
aws ecs update-service --cluster retail-store-ecs-cluster --service ui --task-definition retail-store-ecs-ui
```

ECS에 등록한 Task Definition을 사용해 ECS Service를 업데이트합니다. 

>주의할 점으로 ECS Service에 변경된 설정을 적용하려면 먼저 새로운 Task Definition revision을 등록(register)한 후, 해당 revision을 사용하도록 Service를 업데이트해야 합니다.

---

```bash
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

ECS Service가 안정화될 때까지 기다립니다.

---

이제 브라우저를 새로고침하면 아래와 같은 화면을 확인할 수 있습니다. 일부 텍스트와 버튼의 색상이 파란색에서 초록색으로 변경되었습니다.

![Demo Store New UI](retail-ui-service-new-theme.png)
