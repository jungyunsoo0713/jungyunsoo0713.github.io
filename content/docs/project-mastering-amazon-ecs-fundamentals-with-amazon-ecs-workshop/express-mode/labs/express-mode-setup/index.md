---
title: "Express Mode Setup"
weight: 1
---
> **작성일:** 2026-05-15 | **수정일:** 2026-05-15

이번 섹션에서는 Amazon ECS Express Mode를 사용하여 애플리케이션을 배포하는 첫 번째 준비 작업을 합니다. Express Mode로 애플리케이션을 배포하기 전에 Infrastructure Role이 필요합니다. 이 IAM Role은 ECS Express Mode가 로드 밸런서, 오토 스케일링, 네트워크 설정을 포함한 인프라를 관리하게 허락합니다. AWS 계정에 이 Infrastructure Role이 존재하고 적합한 Managed Policy에 연결되어 있는지 조회하는 것이 이 첫 번째 준비 작업입니다.

다음 명령어를 실행하여 `ecsExpressInfrastructureRole`이라는 IAM Role에 연결된 Managed Policy 목록을 조회합니다. `ecsExpressInfrastructureRole`은 `EcsImmersionDay.json` CloudFormation 템플릿 파일을 사용해 스택을 생성하는 과정에서 이미 생성된 IAM Role입니다.

```bash
echo_c "ecsExpressInfrastructureRole"
aws iam list-attached-role-policies --role-name ecsExpressInfrastructureRole
```

이 명령어의 출력입니다.

```bash
ecsExpressInfrastructureRole
{
  "AttachedPolicies": [
    {
      "PolicyName": "AmazonECSInfrastructureRoleforExpressGatewayServices",
      "PolicyArn": "arn:aws:iam::aws:policy/service-role/AmazonECSInfrastructureRoleforExpressGatewayServices"
    }
  ]
}
```

`ecsExpressInfrastructureRole`에는 `AmazonECSInfrastructureRoleforExpressGatewayServices` Managed Policy가 연결되어 있습니다. `AmazonECSInfrastructureRoleforExpressGatewayServices`는 `ecsExpressInfrastructureRole`에 로드 밸런서, 오토 스케일링, 네트워크 설정을 포함한 인프라를 관리할 권한을 부여하는 IAM Policy입니다.
