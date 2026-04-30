---
title: "Amazon ECS Cluster"
weight: 1
---
> **작성일:** 2026-04-22 | **수정일:** 2026-04-30

이번 섹션에서는 ECS Cluster를 생성합니다. ECS Cluster는 ECS Service와 Task를 실행하고 관리하기 위한 논리적 공간입니다.

```bash
aws ecs create-cluster --cluster-name retail-store-ecs-cluster --settings name=containerInsights,value=enhanced
```

위 명령어는 ECS Cluster를 새로 생성하는 명령어입니다. 

> 참고로 하나의 애플리케이션 안에 여러 ECS 클러스터를 둘 수도 있습니다. 하지만 일반적인 MSA 애플리케이션에서는 하나의 ECS 클러스터 안에 여러 ECS Service를 배치하는 구성이 흔합니다. 클러스터를 여러 개로 나누는 경우는 운영 경계, 보안 경계, 비용 관리, 장애 영향 범위 분리처럼 명확한 이유가 있을 때입니다.

>예를 들면

```bash
>retail-store-frontend-cluster
└─ ui service

retail-store-backend-cluster
├─ catalog service
├─ cart service
└─ orders service

retail-store-batch-cluster
└─ batch task
```

>이런 식으로 여러 개의 클러스터를 한 애플리케이션 안에 둘 수 있습니다.

---

`--cluster-name retail-store-ecs-cluster` 옵션은 클러스터 이름을 `retail-store-ecs-cluster`로 지정합니다.

---

`--settings name=containerInsights,value=enhanced` 옵션은 이 클러스터에서 Container Insights를 `enhanced` 모드로 활성화합니다. Container Insights는 CloudWatch에서 ECS 클러스터, 서비스, 태스크 수준의 메트릭과 모니터링 정보를 더 자세히 확인할 수 있게 해주는 기능입니다.

---

위 명령어가 출력하는 값입니다.

```json
{
  "cluster": {
    "clusterArn": "arn:aws:ecs:ap-northeast-3:802104112480:cluster/retail-store-ecs-cluster",
    "clusterName": "retail-store-ecs-cluster",
    "status": "ACTIVE",
    "registeredContainerInstancesCount": 0,
    "runningTasksCount": 0,
    "pendingTasksCount": 0,
    "activeServicesCount": 0,
    "statistics": [],
    "tags": [],
    "settings": [
      {
        "name": "containerInsights",
        "value": "enhanced"
      }
    ],
    "capacityProviders": [],
    "defaultCapacityProviderStrategy": []
  }
}
```

`"registeredContainerInstancesCount"`는 이 ECS Cluster에 등록된 EC2 컨테이너 인스턴스 수를 의미합니다. 여기서 컨테이너 인스턴스는 ECS 에이전트가 설치되어 클러스터에 등록된 EC2 인스턴스를 뜻합니다. 참고로 Fargate는 EC2 인스턴스를 클러스터에 직접 등록해 사용하는 방식이 아니라 AWS가 관리하는 서버리스 인프라에서 태스크를 실행하는 방식이므로, 이 값에는 카운트되지 않습니다.

---

`"runningTasksCount"`는 현재 실행 중인 태스크 수를 의미합니다. 태스크는 하나 이상의 컨테이너를 포함할 수 있는 실행 단위이며, 이 값에는 EC2 인스턴스에서 실행 중인 태스크뿐 아니라 Fargate에서 실행 중인 태스크도 포함됩니다.

---

`"pendingTasksCount"`는 현재 이 ECS Cluster에서 `PENDING` 상태인 태스크 수를 의미합니다. 즉 아직 실행이 완료된 것은 아니고, 실행을 준비 중인 태스크 수를 나타냅니다.

---

`"activeServicesCount"`는 이 ECS Cluster에서 현재 `ACTIVE` 상태인 ECS Service 개수를 의미합니다. 여기서 `ACTIVE`는 서비스가 현재 유효하게 존재하는 상태를 뜻합니다.

ECS에서 Task는 하나 이상의 컨테이너를 실제로 실행하는 단위입니다. 보통 하나의 메인 컨테이너만 두는 경우도 많지만, 구조에 따라 로그 수집이나 프록시 같은 보조 컨테이너를 함께 둘 수도 있습니다. MSA 애플리케이션에서는 각 마이크로서비스가 하나의 ECS Service에 대응된다고 볼 수 있습니다.

ECS Service는 특정 Task Definition을 기준으로 이러한 Task를 원하는 개수만큼 실행하고 유지하는 역할을 합니다. 따라서 Service는 관리 단위이고 Task는 실행 단위이므로, 둘의 개수는 서로 같지 않을 수 있습니다. 보통 하나의 Service는 고가용성을 위해 동일한 기능을 수행하는 여러 개의 Task를 실행합니다. 따라서 하나의 Service가 관리하는 Task들은 일반적으로 동일한 역할을 수행합니다.

ECS Service 구조 예시:

```txt
ECS Service (예: Retail Store Sample App의 orders)
├── Task 1 (예: AZ-a)
│   ├── Container 1 (orders-app)
│   └── Container 2 (log-router)
├── Task 2 (예: AZ-b)
│   ├── Container 1 (orders-app)
│   └── Container 2 (log-router)
└── Task 3 (예: AZ-c)
    ├── Container 1 (orders-app)
    └── Container 2 (log-router)
```

---

`"statistics"`는 ECS Cluster의 추가 통계 정보를 담는 필드입니다. 이 값에는 launch type별로 구분된 태스크 수와 서비스 수 같은 정보가 포함될 수 있습니다. 다음은 `"statistics"`값의 예시 입니다.

```json
"statistics": [
  {
    "name": "runningEC2TasksCount",
    "value": "2"
  },
  {
    "name": "runningFargateTasksCount",
    "value": "3"
  },
  {
    "name": "activeEC2ServiceCount",
    "value": "1"
  },
  {
    "name": "activeFargateServiceCount",
    "value": "2"
  }
]
```

---

`"capacityProviders"`는 이 클러스터에 연결된 Capacity Provider 목록을 의미합니다. Capacity Provider는 Launch Type과 관련된 개념이지만, Launch Type처럼 단순히 ECS 태스크를 어떤 실행 기반에서 실행할지 정하는 데 그치지 않고, 그에 필요한 컴퓨팅 용량을 어떻게 관리할지도 함께 정하는 방식입니다. 여기서 "필요한 용량"이란 ECS 태스크를 실행하기 위한 컴퓨팅 자원을 뜻합니다. EC2 기반 Capacity Provider에서는 이 용량 관리가 EC2 Auto Scaling과 연결될 수 있지만, Fargate 기반 Capacity Provider에서는 AWS가 서버 인프라를 직접 관리합니다. 예를 들어 `"FARGATE"`, `"FARGATE_SPOT"`, 또는 사용자가 직접 생성한 EC2 Auto Scaling group 기반 Capacity Provider를 사용할 수 있습니다. 

다음은 `"capacityProviders"` 값의 예시입니다.

```json
"capacityProviders": [
  "FARGATE",
  "FARGATE_SPOT",
  "MyAsgCapacityProvider"
]
```

참고로 클러스터의 `"capacityProviders"`에는 Fargate 계열과 EC2 Auto Scaling group 계열 Capacity Provider를 함께 연결할 수 있습니다. 다만 실제 태스크 배치에 사용되는 `"capacityProviderStrategy"` 또는 `"defaultCapacityProviderStrategy"` 안에서는 같은 계열의 Capacity Provider만 함께 사용할 수 있습니다.

Launch Type과 Capacity Provider를 비교하자면, Launch Type은 ECS 태스크를 어떤 인프라에서 실행할지 직접 지정하는 방식입니다. 반면 Capacity Provider는 태스크를 어떤 실행 기반에서 실행할지뿐만 아니라, 여러 실행 기반 사이에 어떻게 분배하고 용량을 어떻게 관리할지까지 포함하는 더 유연한 방식입니다. AWS는 일반적으로 Launch Type보다 Capacity Provider 사용을 권장합니다.

---

`"defaultCapacityProviderStrategy"`는 이 ECS Cluster에서 기본으로 사용할 Capacity Provider 전략을 의미합니다. 여기에는 어떤 Capacity Provider를 사용할지와, 여러 Capacity Provider를 함께 사용할 경우 태스크를 어떻게 분배할지에 대한 설정이 들어갈 수 있습니다. 서비스나 태스크를 실행할 때 `"launchType"`이나 `"capacityProviderStrategy"`를 별도로 지정하지 않으면 이 값이 기본으로 적용됩니다. 

다음은 `"defaultCapacityProviderStrategy"`값의 예시 입니다.

```json
"defaultCapacityProviderStrategy": [
  {
    "capacityProvider": "MyAsgCapacityProviderA",
    "weight": 1,
    "base": 1
  },
  {
    "capacityProvider": "MyAsgCapacityProviderB",
    "weight": 4,
    "base": 0
  }
]
```

여기서 `"base"`는 여러 Capacity Provider 중 해당 Capacity Provider에 먼저 배정할 최소 태스크 수를 의미합니다. `"weight"`는 이 최소 수량을 반영한 뒤, 남은 태스크를 여러 Capacity Provider 사이에 어떤 비율로 분배할지 정하는 값입니다. 위 예시에서는 첫 번째 Capacity Provider인 `"MyAsgCapacityProviderA"`에 최소 1개의 태스크가 먼저 배정됩니다. 그 이후 남은 태스크들은 `"MyAsgCapacityProviderA"`와 `"MyAsgCapacityProviderB"`에 `1:4` 비율로 분배됩니다.

---

>AWS CLI로 `aws ecs create-cluster` 명령어를 실행해서 생성한 ECS Cluster는 CloudFormation 스택에 자동으로 반영되지 않습니다. AWS CLI를 통해 생성한 리소스는 기본적으로 스택에 자동으로 포함되지 않습니다. 해당 리소스를 스택에 포함하려면 먼저 CloudFormation 템플릿에 그 리소스를 정의해야 한 뒤. Resource Import 작업을 통해 스택에 가져와야 합니다.

>예를 들면:

```json
"RetailStoreEcsCluster": {
  "Type": "AWS::ECS::Cluster",
  "DeletionPolicy": "Retain",
  "Properties": {
    "ClusterName": "retail-store-ecs-cluster",
    "ClusterSettings": [
      {
        "Name": "containerInsights",
        "Value": "enhanced"
      }
    ]
  }
}
```

> 이런 코드를 템플릿에 작성해 준 후. 

```txt
CloudFormation
→ Stacks
→ 기존 스택 선택
→ Stack actions
→ Import resources into stack
→ 수정한 템플릿 업로드
→ 가져올 리소스 선택
→ Identifier에 실제 ClusterName 입력
→ Review
→ Import resources (여기서 실제 스택이 업데이트 됩니다.)
```

위 과정이 스택을 업데이트 하는 과정입니다.
