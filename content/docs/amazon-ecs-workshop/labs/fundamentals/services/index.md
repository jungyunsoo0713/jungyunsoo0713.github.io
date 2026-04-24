---
title: "Services"
weight: 4
draft: true
---
> **작성일:** 2026-04-23 | **수정일:** 2026-04-23
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

`--desired-count`는 이 서비스가 유지하려는 태스크 개수를 의미합니다. 여기서는 2개의 태스크가 항상 실행된 상태로 유지되도록 설정합니다.

---

`--load-balancers`는 이 서비스의 태스크를 어떤 로드 밸런서와 연결할지 정하는 설정입니다. 여기서는 이 서비스의 태스크를 `${UI_TG_ARN}` 타깃 그룹에 등록하고, 태스크 정의 안에 있는 `application` 컨테이너의 `8080` 포트를 로드 밸런서가 트래픽을 전달할 대상으로 사용하도록 설정합니다. 참고로 `${UI_TG_ARN}` 값은 `BasicBlueTargetGroup17B9FA9D` 타깃 그룹의 ARN입니다.

---

`--network-configuration`은 이 서비스의 태스크에 붙이는 네트워크 설정입니다. 여기서는 태스크를 `${PRIVATE_SUBNET1}`, `${PRIVATE_SUBNET2}` 프라이빗 서브넷에 배치하고, `${UI_SG_ID}` 보안 그룹을 적용하며, 퍼블릭 IP는 할당하지 않도록 설정합니다.`

`${PRIVATE_SUBNET1}`과 `${PRIVATE_SUBNET2}`는 이 서비스의 태스크를 배치할 프라이빗 서브넷의 Subnet ID를 저장한 변수입니다. 템플릿상으로는 각각 `BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4`, `BasicRetailStoreVPCPrivateSubnet2Subnet5E255638` 리소스에 해당하며, `10.0.3.0/24`, `10.0.4.0/24` 대역의 프라이빗 서브넷입니다. `${UI_SG_ID}`는 UI 태스크에 적용할 보안 그룹의 Security Group ID를 저장한 변수이며, 템플릿상으로는 `BasicUISecurityGroup41F5FEAF` 리소스에 해당합니다. 이 보안 그룹은 `8080` 포트 인바운드 트래픽을 허용하도록 설정되어 있습니다. 즉 `${PRIVATE_SUBNET1}`, `${PRIVATE_SUBNET2}`는 태스크를 어느 서브넷에 둘지 정하는 값이고, `${UI_SG_ID}`는 그 태스크에 어떤 보안 그룹을 적용할지 정하는 값입니다.

