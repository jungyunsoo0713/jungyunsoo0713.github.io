---
title: "Scheduled Scaling"
weight: 4
---
> **작성일:** 2026-05-16 | **수정일:** 2026-05-16

이번 섹션에서는 ECS Service Auto Scaling에 Scheduled Action을 설정하는 법을 알아봅니다. Scheduled Action은 Scheduled Scaling을 구현하기 위해 생성하는 예약 작업입니다.

---

```bash
aws application-autoscaling register-scalable-target \
    --service-namespace ecs \
    --scalable-dimension ecs:service:DesiredCount \
    --resource-id service/retail-store-ecs-cluster/ui \
    --min-capacity 2 \
    --max-capacity 10
```

위 명령어로 ECS 서비스를 Application Auto Scaling의 스케일링 대상으로 이미 등록하였습니다. 다음 명령어를 실행하여 ECS 서비스에 Scheduled Action을 적용합니다. 

```bash
aws application-autoscaling put-scheduled-action \
 --service-namespace ecs \
 --scalable-dimension ecs:service:DesiredCount \
 --resource-id service/retail-store-ecs-cluster/ui \
 --scheduled-action-name "test-scale-up" \
 --schedule "at($(date -u -d "+1 minutes" "+%Y-%m-%dT%H:%M:%S"))" \
 --scalable-target-action MinCapacity=5,MaxCapacity=10
```

`--schedule` 옵션은 이 Scheduled Scaling이 작동하는 시간을 설정합니다. 여기서는 현재 시각에서 1분 뒤에 작동하도록 설정합니다.

`--scalable-target-action`은 Scheduled Action이 실행될 때, 스케일링 범위를 어떻게 조절할지 정하는 옵션입니다. 여기서는 최소 태스크 수는 5개, 최대 태스크 수는 10개로 지정합니다.

>실무에서는 `--schedule "cron(0 9 ? * MON-FRI *)"` 이런 식으로 특정 시간대를 정해줍니다.

---

다음 명령어를 실행하여 Scheduled Action이 실행될 시간을 기다립니다. 이 명령어는 80초 동안 대기한 뒤, 다음 명령어를 실행할 수 있다는 메시지를 출력합니다.

```bash
echo "Starting timer for 1 plus minutes..." && sleep 80 && echo "Time's up! and you can run the next command"
```

다음 명령어를 실행하여 현재 스케일 아웃 중인 태스크를 포함한 UI 서비스의 태스크들을 확인할 수 있습니다.

```bash
aws ecs describe-tasks \
 --cluster retail-store-ecs-cluster \
 --tasks $(aws ecs list-tasks --cluster retail-store-ecs-cluster --service-name ui --query 'taskArns[]' --output text) \
 --query "tasks[*].[group, launchType, lastStatus, healthStatus, taskArn]" --output table
```

이 명령어의 출력입니다.

![aws-ecs-describe-tasks 명령어 출력](aws-ecs-describe-tasks.png)

`ACTIVATING`으로 표시된 Fargate 시작 유형 태스크들이 스케일 아웃 중인 태스크입니다. 현재는 모두 `RUNNING`이기 때문에 스케일 아웃이 완료되어 모든 태스크가 실행 중입니다.

---

다음 명령어를 실행하여 현재 ECS 서비스가 안정화될 때까지 기다린 후, 서비스의 원하는 태스크 수를 `2`로 변경하여 업데이트합니다. 이를 통해 스케일 아웃 시 생성된 태스크의 수를 다시 줄일 수 있습니다.

```bash
aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui

aws ecs update-service \
    --cluster retail-store-ecs-cluster \
    --service ui \
    --task-definition retail-store-ecs-ui \
    --desired-count 2
```



