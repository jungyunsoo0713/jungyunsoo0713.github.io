---
title: "Services"
weight: 3
---
> **작성일:** 2026-04-23 | **수정일:** 2026-04-30

이번 섹션에서는 ECS 서비스를 생성해 애플리케이션 태스크를 안정적으로 실행하고 유지합니다. ECS 서비스는 원하는 개수의 태스크를 계속 실행하도록 관리하며, 태스크 장애 시 새 태스크를 다시 실행합니다. 또한 로드 밸런서와 연동해 여러 태스크로 트래픽을 분산하고 애플리케이션의 고가용성을 높입니다.

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

`--desired-count`는 이 서비스가 유지하려는 태스크 개수를 의미합니다. 태스크에 장애가 발생하거나 태스크가 중지되면, ECS Service는 새로운 태스크를 자동으로 생성해 원하는 개수를 맞춥니다. 여기서는 2개의 태스크가 항상 실행된 상태로 유지되도록 설정합니다.

>`aws ecs run-task`를 사용하면 Service를 만들지 않고 Task Definition을 기준으로 Task만 직접 실행할 수 있습니다. 반면 `aws ecs create-service`를 사용하면 Task Definition을 지정해 Service를 생성할 수 있습니다. 이때 Service는 지정된 Task Definition을 바탕으로 원하는 개수의 Task를 실행하고 유지합니다. 
>
>`aws ecs run-task`는 ECS Service가 먼저 있어야 하는 명령어가 아닙니다. Cluster와 Task Definition만 있으면 Service 없이도 Task를 직접 실행할 수 있습니다. 다만 Task가 실행될 ECS Cluster는 반드시 필요합니다. 이 명령어는 ECS Service 없이 Task만 띄우는 명령어 입니다.

```bash
aws ecs run-task \
  --cluster retail-store-ecs-cluster \
  --task-definition retail-store-ecs-ui \
  --count 1 \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[${PRIVATE_SUBNET1},${PRIVATE_SUBNET2}],securityGroups=[${UI_SG_ID}],assignPublicIp=DISABLED}"
```

>Service 없이 Task만 실행할 때는 ECS Service, Load Balancer, Target Group, desired count 설정이 필요 없습니다. 대신 Task가 실행될 Cluster, 실행할 Task Definition, 그리고 Fargate 네트워크 설정은 필요합니다.
>
>Service 없이 Task만 실행하는 경우도 흔합니다. 계속 실행되어야 하는 웹 애플리케이션이나 API 서버는 보통 ECS Service로 운영합니다. 이는 고가용성, 트래픽 분산, 장애 대응이 필요하기 때문입니다. 반면 배치 작업, 데이터 마이그레이션, DB 초기화, 정기 작업처럼 한 번 실행하고 종료되는 작업은 `aws ecs run-task`로 Task만 직접 실행하는 경우가 많습니다.

---

`--load-balancers`는 이 서비스의 태스크를 어떤 로드 밸런서와 연결할지 정하는 설정입니다. 여기서는 이 서비스의 태스크를 `${UI_TG_ARN}` 타깃 그룹에 등록하고, 태스크 정의 안에 있는 `application` 컨테이너의 `8080` 포트를 로드 밸런서가 트래픽을 전달할 대상으로 사용하도록 설정합니다. 참고로 `${UI_TG_ARN}` 값은 `BasicBlueTargetGroup17B9FA9D` 타깃 그룹의 ARN입니다.

---

`--network-configuration`은 이 서비스의 태스크에 붙이는 네트워크 설정입니다. 여기서는 태스크를 `${PRIVATE_SUBNET1}`, `${PRIVATE_SUBNET2}` 프라이빗 서브넷에 배치하고, `${UI_SG_ID}` 보안 그룹을 적용하며, 퍼블릭 IP는 할당하지 않도록 설정합니다.`

`${PRIVATE_SUBNET1}`과 `${PRIVATE_SUBNET2}`는 이 서비스의 태스크를 배치할 프라이빗 서브넷의 Subnet ID를 저장한 변수입니다. 템플릿상으로는 각각 `BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4`, `BasicRetailStoreVPCPrivateSubnet2Subnet5E255638` 리소스에 해당하며, `10.0.3.0/24`, `10.0.4.0/24` 대역의 프라이빗 서브넷입니다. `${UI_SG_ID}`는 UI 태스크에 적용할 보안 그룹의 Security Group ID를 저장한 변수이며, 템플릿상으로는 `BasicUISecurityGroup41F5FEAF` 리소스에 해당합니다. 이 보안 그룹은 `8080` 포트 인바운드 트래픽을 허용하도록 설정되어 있습니다. 즉 `${PRIVATE_SUBNET1}`, `${PRIVATE_SUBNET2}`는 태스크를 어느 서브넷에 둘지 정하는 값이고, `${UI_SG_ID}`는 그 태스크에 어떤 보안 그룹을 적용할지 정하는 값입니다.

---

```bash
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

ECS 서비스가 안정 상태가 될 때까지 기다리는 명령어입니다. ECS Service를 생성하거나 업데이트한 직후에는 Task 생성, 컨테이너 실행, 상태 검사, 로드 밸런서 등록 등의 과정이 아직 진행 중일 수 있습니다. 이 명령어를 사용하면 Service가 안정화되기 전까지 다음 단계로 넘어가지 않도록 할 수 있습니다.

`--cluster retail-store-ecs-cluster`는 어떤 ECS 클러스터의 서비스를 확인할지 지정합니다.

`--services ui` 그 클러스터 안에서 어떤 ECS 서비스를 기다릴지 지정합니다. 여기서는 `ui` 서비스입니다.

---

```bash
aws ecs describe-tasks \
    --cluster retail-store-ecs-cluster \
    --tasks $(aws ecs list-tasks --cluster retail-store-ecs-cluster --query 'taskArns[]' --output text) \
    --query "tasks[*].[group, launchType, lastStatus, healthStatus, taskArn]" --output table
```

위 명령어는 `retail-store-ecs-cluster` 클러스터에서 실행 중인 Task 목록을 조회한 뒤, 각 Task의 상세 정보를 확인합니다. 내부의 `aws ecs list-tasks` 명령어로 Task ARN 목록을 가져오고, 그 결과를 `aws ecs describe-tasks`의 `--tasks` 값으로 전달합니다. 이후 `--query` 옵션을 사용해 Task의 그룹, 실행 방식, 현재 상태, 헬스 상태, Task ARN만 추려 표 형태로 출력합니다.

위 명령어의 출력입니다.

```bash
-----------------------------------------------------------------------------------------------------------------------------------------------------------
|                                                                      DescribeTasks                                                                      |
+------------+----------+----------+----------+-----------------------------------------------------------------------------------------------------------+
|  service:ui|  FARGATE |  RUNNING |  HEALTHY |  arn:aws:ecs:ap-northeast-3:802104112480:task/retail-store-ecs-cluster/3e7a1395d08a42a89371db1f2755625e   |
|  service:ui|  FARGATE |  RUNNING |  HEALTHY |  arn:aws:ecs:ap-northeast-3:802104112480:task/retail-store-ecs-cluster/5e1c383c0e4a4d4a8beb0b98276134a2   |
+------------+----------+----------+----------+-----------------------------------------------------------------------------------------------------------+
```

---

```bash
export RETAIL_ALB=$(aws elbv2 describe-load-balancers --name retail-store-ecs-ui \
 --query 'LoadBalancers[0].DNSName' --output text)

echo_c http://${RETAIL_ALB} ; echo
```

위 명령어는 `retail-store-ecs-ui` Application Load Balancer의 DNS 이름을 조회해 `RETAIL_ALB` 환경 변수에 저장합니다. 이후 `http://${RETAIL_ALB}` 형태로 URL을 출력하여 브라우저에서 접속할 주소를 확인할 수 있게 합니다.

위 명령어의 출력입니다.

```bash
http://retail-store-ecs-ui-1878288695.ap-northeast-3.elb.amazonaws.com
```

이 주소를 브라우저의 주소창에 입력한 후 접속하면, 아래의 화면이 나옵니다.

![Demo Store UI](ui-service-webapp-nocatalog-noasset.png)



