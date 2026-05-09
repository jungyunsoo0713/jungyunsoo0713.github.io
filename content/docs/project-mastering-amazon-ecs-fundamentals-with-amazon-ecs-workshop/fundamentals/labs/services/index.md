---
title: "Services"
weight: 3
---
> **작성일:** 2026-05-09 | **수정일:** 2026-05-09

이번 섹션에서는 이전에 생성한 Task Definition을 사용하여 ECS 서비스를 ECS 클러스터에 생성합니다. 

---

ECS 서비스를 생성하는 명령어 입니다.

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

`--cluster`는 이 ECS 서비스를 생성할 클러스터를 지정합니다. 이전에 생성한 ECS 클러스터인 `retail-store-ecs-cluster`를 지정합니다.

---

`--service-name`는 이 ECS 서비스의 이름을 지정합니다. 이 ECS 서비스의 이름을 `ui` 로 지정합니다.

---

`--task-definition`은 이 ECS 서비스를 생성할 때 사용할 Task Definition을 지정합니다. 이전에 ECS에 등록한 Task Definition의 `family`인 `retail-store-ecs-ui`를 사용합니다. `--task-definition`의 값으로는 Task Definition의 `family` 값이나 `family:revision` 형식, 또는 Task Definition의 ARN이 올 수 있습니다. 여기서 `family:revision`은 특정 `family`의 특정 버전을 의미합니다.

---

`--desired-count 2`는 ECS 서비스가 유지하려는 태스크 수를 두 개로 지정합니다.

---

`--launch-type FARGATE`은 이 ECS 서비스의 태스크들이 Fargate Launch Type(실행 환경)으로 실행된다는 것을 의미합니다.

---

`--health-check-grace-period-seconds 30`는 이 ECS 서비스의 각 태스크 시작 후 30초 동안 ECS 서비스 스케줄러가 태스크 상태 판단에 사용되는 모든 종류의 헬스 체크가 실패해도 실패로 간주하지 않겠다는 것을 의미합니다. ECS 서비스 스케줄러는 ECS 서비스의 태스크 수와 상태를 관리하는 ECS의 스케줄링 로직입니다. 태스크 상태 판단에 사용되는 헬스 체크는 ELB 헬스 체크, VPC Lattice 헬스 체크, 컨테이너 헬스 체크가 있습니다.

주의할 점은, 앞서 작성한 Task Definition에 있는 `"healthCheck.startPeriod"`는 컨테이너의 헬스 체크 실패를 무시하는 것이고, `--health-check-grace-period-seconds`는 ECS 서비스 스케줄러가 태스크 상태 판단에 사용되는 모든 종류의 헬스 체크 실패를 무시하는 것이라는 점입니다.

---

`--load-balancers`는 이 ECS 서비스가 실행하는 태스크가 어떤 타깃 그룹에 등록되는지를 지정합니다. `targetGroupArn=${UI_TG_ARN}`으로 타깃 그룹의 ARN을 지정합니다. `containerName=application`으로 타깃 그룹에 등록될 태스크의 컨테이너의 이름을 지정합니다.
이전에 생성한 Task Definition의 `"containerDefinitions"`에 `"name": "application"`으로 태스크의 컨테이너 이름을 지정하였습니다. 

>리마인드 하자면, 타깃 그룹에는 일반적으로 여러 동일한 ECS 서비스의 태스크들이 등록됩니다.

>Task Definition에 `"name": "application"`과 `"containerPort": 8080`은 컨테이너의 이름과 포트를 지정하는 것이고, `aws ecs create-service` 명령어의 `--load-balancers` 옵션의 값 `containerName=application`과 `containerPort=8080`은 로드 밸런서가 트래픽을 어떤 컨테이너의 포트에 보낼지를 알려주는 것입니다.

---

`--network-configuration`은 이 ECS 서비스의 태스크들이 배치될 서브넷들과 보안 그룹 ID, 퍼블릭 IP 할당 여부 등을 지정합니다. `assignPublicIp=DISABLED`는 이 서비스의 태스크가 퍼블릭 IP를 자동으로 할당받지 않는다는 것을 의미합니다.

---

위 ECS 서비스를 생성하는 명령어를 실행한 후 다음 명령어를 사용하여 ECS 서비스가 안정화될 때까지 기다립니다.

```bash
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

`aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui` 명령어 뒤에 온 같은 명령어 흐름에 있는 명령어는, 이 명령어가 끝나기 전까지는 실행 대기 상태로 있습니다. 

---

다음 명령어로 현재 실행 중인 태스크를 볼 수 있습니다.

```bash
aws ecs describe-tasks \
    --cluster retail-store-ecs-cluster \
    --tasks $(aws ecs list-tasks --cluster retail-store-ecs-cluster --query 'taskArns[]' --output text) \
    --query "tasks[*].[group, launchType, lastStatus, healthStatus, taskArn]" --output table
```

이 명령어의 출력입니다.

```
-----------------------------------------------------------------------------------------------------------------------------------------------------------
|                                                                      DescribeTasks                                                                      |
+------------+----------+----------+----------+-----------------------------------------------------------------------------------------------------------+
|  service:ui|  FARGATE |  RUNNING |  HEALTHY |  arn:aws:ecs:ap-northeast-3:802104112480:task/retail-store-ecs-cluster/3e7a1395d08a42a89371db1f2755625e   |
|  service:ui|  FARGATE |  RUNNING |  HEALTHY |  arn:aws:ecs:ap-northeast-3:802104112480:task/retail-store-ecs-cluster/5e1c383c0e4a4d4a8beb0b98276134a2   |
+------------+----------+----------+----------+-----------------------------------------------------------------------------------------------------------+
```

UI 서비스의 태스크 두 개가 실행 중인 것을 확인할 수 있습니다.

---

다음 명령어로 현재 실행 중인 서비스에서 사용 중인 로드 밸런서의 URL을 가져올 수 있습니다. ALB가 클라이언트의 요청을 받는 주체이기 때문에, 이 ALB의 DNS 이름이 곧 이 애플리케이션에 접속할 주소가 됩니다.

```bash
export RETAIL_ALB=$(aws elbv2 describe-load-balancers --name retail-store-ecs-ui \
 --query 'LoadBalancers[0].DNSName' --output text)

echo_c http://${RETAIL_ALB} ; echo
```

이 명령어의 출력입니다.

```bash
http://retail-store-ecs-ui-1878288695.ap-northeast-3.elb.amazonaws.com
```

이 주소를 브라우저의 주소창에 입력한 후 접속하면, 아래의 화면이 나옵니다.

![Demo Store UI](ui-service-webapp-nocatalog-noasset.png)

---

다음은 현재 ECS의 구조입니다. 

![Retail UI Service - VPC Architecture](retail-ui-service.svg)
