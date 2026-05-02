---
title: "EcsImmersionDay.json CloudFormation 템플릿 소개"
weight: 1
---
> **작성일:** 2026-05-02 | **수정일:** 2026-05-02
## EcsImmersionDay.json CloudFormation 템플릿 소개

Amazon ECS Workshop에서는 EcsImmersionDay.json CloudFormation 템플릿을 제공합니다. 이 템플릿은 랩 실습에 필요한 모든 인프라를 한 번에 배포할 수 있게 해줍니다. 그 후 각 랩에서는 이 인프라를 바탕으로 추가적인 인프라를 설치하는 것을 실습하게 됩니다.

이 템플릿의 코드를 뜯어보는 것은 AWS 핵심 인프라를 학습하는 데 큰 도움이 됩니다. 각각의 인프라가 어떻게 연결되어 있는지를 알 수 있고, 세부 설정이 어떻게 되어 있는지도 파악할 수 있습니다.

## EcsImmersionDay.json 템플릿 내부 구조 소개

```json
{
 "Description": "ECS Immersion Day Workshop Stack - Created on 2026-03-31 - Version 1.0.4",
 "Metadata": {...
 },
 "Mappings": {...
 },
 "Resources": {...
 },
 "Parameters": {...
 },
 "Outputs": {...
 }
}
```

EcsImmersionDay.json CloudFormation 템플릿은 위와 같은 구조를 가지고 있습니다. 이 중 Resources에는 랩 실습에서 사용될 인프라 리소스들에 대한 정보가 담겨 있습니다. 예를 들면,

```json
  "BasicRetailStoreVPCPublicSubnet1EIP375531A2": {
   "Type": "AWS::EC2::EIP",
   "Properties": {
    "Domain": "vpc",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.4"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   }
  },
```

이런 JSON 코드가 존재합니다.

이 템플릿에는 약 150개의 리소스가 존재하고, 각 리소스은 prefix를 가지고 있습니다. 위 리소스의 prefix는 Basic으로, Fundamentals 랩 실습의 기반이 되는 리소스입니다.

또 다른 예시로, 아래 리소스는 Network prefix를 가지고 있고, Networking 랩 실습의 기반이 되는 리소스입니다.

```json
  "NetworkRetailStoreVPC2PublicSubnet1RouteTableAssociation370D0C06": {

   "Type": "AWS::EC2::SubnetRouteTableAssociation",

   "Properties": {

    "RouteTableId": {

     "Ref": "NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426"

    },

    "SubnetId": {

     "Ref": "NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3"

    }

   }

  },
```

즉, 각 랩 실습의 기반이 되는 리소스들이 있고, 그 리소스들은 각기 다른 prefix를 가지고 있습니다. 참고로 Fundamentals 랩에서 사용되는 Basic prefix를 가진 리소스들은 다른 랩에서도 기반이 되는 리소스들입니다.

이 프로젝트는 각 랩 실습을 진행하기 전에 각 랩의 기반이 되는 prefix를 가진 리소스들을 먼저 분석합니다. 이를 통해 각 랩 실습 진행에 필요한 리소스들이 왜 필요한지, 어떤 기능을 하는지 파악할 수 있습니다.
