---
title: "Auto Scaling - Labs"
weight: 2
---
> **작성일:** 2026-05-16 | **수정일:** 2026-05-16

Fargate 시작 유형에서는 하나의 태스크가 독립적인 환경을 가지고 실행됩니다. Fargate는 서버리스이기 때문에, 사용자는 EC2 인스턴스 타입을 지정하지 않지만 Task Definition에 CPU와 메모리 값을 정해주어야 합니다.

Fargate 시작 유형에서 태스크에 적절한 CPU와 메모리 값을 설정했다면, 외부 요청이 증가함에 따라 태스크 수를 증가시킴으로써 수평 확장(Horizontal Scaling)을 할 수 있습니다.

Amazon ECS는 서비스에서 원하는 태스크 수를 자동을 맞추는 기능을 제공합니다. 이 기능을 Service Auto Scaling이라고 부릅니다. 오토 스케일링을 효과적으로 하려면 부하에 거의 정비례하는 메트릭(Proportional Metrics)을 기준으로 하는 것이 좋습니다. 예를 들면, 애플리케이션에 가해지는 부하가 두 배가 되었을 때, 해당 메트릭도 두 배가 된다면 이 메트릭을 사용하는 것이 좋습니다.

ECS는 Application Auto Scaling을 사용합니다. Application Auto Scaling은 여러 AWS 리소스들의 용량을 자동으로 스케일링하는 서비스입니다. ECS 서비스의 태스크 수뿐만 아니라 DynamoDB의 읽기/쓰기 용량 등에도 적용됩니다.
