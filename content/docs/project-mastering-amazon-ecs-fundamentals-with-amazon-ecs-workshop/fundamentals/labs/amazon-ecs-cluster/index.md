---
title: "Amazon ECS Cluster"
weight: 1
---

이번 랩에서는 ECS 클러스터, ECS 서비스, Task Definition을 포함한 ECS의 핵심 컴포넌트들을 생성합니다. 이번 랩의 목표는 ALB 뒤에서 작동하는 컨테이너들을 배포하는 것입니다.

이번 섹션에서는 `retail-store-ecs-cluster`라는 이름을 가진 ECS 클러스터를 생성합니다.

---

다음은 ECS 클러스터를 생성하는 명령어 입니다.

```bash
aws ecs create-cluster --cluster-name retail-store-ecs-cluster --settings name=containerInsights,value=enhanced
```

`--settings name=containerInsights,value=enhanced` 옵션은 이 ECS 클러스터가 CloudWatch의 향상된 Container Insights 기능을 사용하게 해주는 옵션입니다. `value`에는 `enhanced` 말고도 `enabled`와 `disabled`라는 값도 올 수 있는데, `enabled` 값은 일반적인 Container Insights 기능을 사용한다는 의미이고 `disabled`는 이 기능을 비활성화한다는 의미입니다.

주의할 점으로는 이 명령어는 ECS 클러스터를 생성하는 것이지, EC2 기반 컨테이너 인스턴스나 Fargate 태스크를 생성한 것은 아니라는 점입니다.

다음은 이 명령어의 출력입니다. 

```bash
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

`"status": "ACTIVE"`는 이 클러스터가 활성화된 상태라는 것을 의미합니다. 

---

`"registeredContainerInstancesCount"`는 이 클러스터에 등록된 EC2 기반 컨테이너 인스턴스의 수를 의미합니다. Fargate 실행 환경에서는 이 값이 `0`으로 집계됩니다. Fargate는 AWS가 완전 관리하는 서버리스 실행 환경에서 실행됩니다. Fargate 태스크는 ECS 클러스터에서 실행되지만, 사용자가 직접 등록하고 관리하는 EC2 기반 컨테이너 인스턴스를 사용하지 않기 때문에 컨테이너 인스턴스로 집계되지 않습니다. 여기서는 실행중인 EC2 기반 컨테이너 인스턴스가 없습니다.

직관적으로 말하자면, `"registeredContainerInstancesCount"`는 이 ECS 클러스터에 등록된 EC2 기반 컴퓨터(서버)가 몇 대인지를 표시한다고 볼 수 있습니다.

>용어 정리를 하자면, EC2 기반 컨테이너 인스턴스와 컨테이너는 다른 것입니다. EC2 기반 컨테이너 인스턴스는 ECS 클러스터에 등록되어 있는 EC2 인스턴스이고, 컨테이너는 ECS 태스크 안에서 실행되는 실제 컨테이너입니다. 태스크 안에서 실행되는 컨테이너는 컨테이너 인스턴스가 아니라, 그냥 컨테이너라고 부릅니다.

---

`"runningTasksCount"`는 현재 실행 중인 태스크의 수를 의미합니다. 값이 `0`이기 때문에 현재 실행 중인 태스크는 없습니다.

---

`"pendingTasksCount"`는 현재 실행 대기 중인 태스크의 수를 의미합니다. 값이 `0`이기 때문에 현재 실행 대기 중인 태스크는 없습니다.

---

`"activeServicesCount"`는 현재 활성 상태인 ECS 서비스의 수를 의미합니다. 값이 `0`이기 때문에 현재 활성 상태인 서비스는 없습니다.

---

`"statistics"`는 태스크와 서비스 수를 Launch Type별로 나눠서 보여주는 통계입니다.

다음은 예시입니다.
```bash
"statistics": [
  {
    "name": "runningEC2TasksCount",
    "value": "0"
  },
  {
    "name": "runningFargateTasksCount",
    "value": "2"
  },
  {
    "name": "activeEC2ServiceCount",
    "value": "0"
  },
  {
    "name": "activeFargateServiceCount",
    "value": "1"
  }
]
```

---

`"tags"`는 이 ECS 클러스터에 등록된 태그들을 보여줍니다.

---

`"settings"`는 이 ECS 클러스터에 적용된 설정들을 보여줍니다. 현재는 향상된 Container Insights를 사용하는 것이 설정되어 있음을 알 수 있습니다.

---

`"capacityProviders"`는 이 ECS 클러스터에서 사용할 수 있는 Capacity Provider 목록을 보여줍니다. Capacity Provider는 태스크의 실행 기반으로, 태스크를 어디에서 실행할지(예: EC2 Auto Scaling Group, Fargate, Fargate Spot 등)를 정하고, 그 실행 기반의 운영 전략을 설정하는 데 사용됩니다. 현재는 사용할 수 있는 Capacity Provider가 없습니다.

다음은 `capacityProviders` 필드의 예시 값입니다.

```bash
"capacityProviders": [
  "FARGATE",
  "FARGATE_SPOT",
  "my-ec2-capacity-provider"
]
```

`"capacityProviders"`는 실행 기반(Capacity Provider)을 표시하고, `capacityProviderStrategy`에서는 이 실행 기반에 대한 운영 전략을 설정합니다.

>Launch Type과 실행 기반(Capacity Provider)은 비슷하지만 약간 의미가 다릅니다. 실행 기반은 태스크를 어디에서 실행할지 정하는 Launch Type일 수도 있고, Launch Type보다 더 세부적인 설정이 가능한 Capacity Provider일 수도 있습니다.

---

`"defaultCapacityProviderStrategy"`는 Capacity Provider의 기본 운영 전략을 보여줍니다. 별도의 Capacity Provider 전략(`capacityProviderStrategy`)을 지정하지 않았을 때, 이 필드의 값이 기본 운영 전략으로 사용됩니다.

다음은 위의 `"capacityProviders"` 예시에 대한 `capacityProviderStrategy` 필드의 예시 값입니다.

```json
"capacityProviderStrategy": [
  {
    "capacityProvider": "FARGATE",
    "base": 1,
    "weight": 1
  },
  {
    "capacityProvider": "FARGATE_SPOT",
    "weight": 2
  },
  {
    "capacityProvider": "my-ec2-capacity-provider",
    "weight": 1
  }
]
```

첫 번째 Capacity Provider인 `"FARGATE"`를 보면 `"base"`와 `"weight"`의 값이 모두 `1`입니다. `"base"`와 `"weight"`는 여러 Capacity Provider를 같이 사용할 때, 태스크를 어떻게 나누어서 배치할지에 대한 설정입니다. 

`"base"`는 이 Capacity Provider에 배치될 최소 태스크 수를 의미합니다. 이 값은 오직 하나의 Capacity Provider에만 설정할 수 있습니다. 이는 특정 Capacity Provider에 우선적으로 태스크를 배치하도록 보장합니다. 주의할 점으로는 `"base"`는 ECS 클러스터의 서비스에서 실행되는 최소 태스크의 수가 아니라는 점입니다. ECS 서비스에서 실행할 태스크 수는 서비스를 생성할 때 명령어(`aws ecs create-service`)의 옵션인 `--desired-count`에서 지정합니다.

`"weight"`는 `"base"` 값으로 특정 Capacity Provider에 태스크가 배치된 후, 남은 태스크들을 어떤 비율로 Capacity Provider들에게 분배할지 정하는 값입니다. 만약 이 비율에 맞지 않는 수의 태스크가 남는다면, ECS 스케줄러가 이를 자동으로 결정합니다. 

예를 들면, 두 개의 태스크가 있다면, 하나는 `"base": 1`이 설정되어 있는 Capacity Provider인 Fargate로 배치되고, 남은 하나는 ECS 스케줄러가 어떤 Capacity Provider에 배치할지 결정합니다. 이 경우는 Fargate Spot Capacity Provider의 가중치(`"weight"`)가 가장 높기 때문에 이 Capacity Provider로 배치될 가능성이 높다고 추측할 수 있습니다.
