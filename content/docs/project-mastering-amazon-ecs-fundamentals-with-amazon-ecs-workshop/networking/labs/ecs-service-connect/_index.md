---
title: "ECS Service Connect"
weight: 1
---
ECS Service Connect는 ECS 서비스 간 통신에 사용되는 기능입니다. 이 기능은 ECS 서비스를 긴 엔드포인트나 복잡한 주소 대신, 짧은 DNS 이름이나 별칭(Alias)으로 접근할 수 있게 해줍니다. 이 DNS 이름이나 별칭은 ECS 클러스터 내부에서 사용될 수 있고, 네트워크 연결과 관련 설정이 되어 있다면 ECS 클러스터 간, VPC 간, 혹은 같은 AWS Region 안의 AWS 계정 간에도 사용될 수 있습니다.

>주의할 점으로, VPC의 설정인 `"EnableDnsSupport": true`와 ECS Service Connect 기능의 작동 여부는 관계가 없다는 점입니다. `"EnableDnsSupport": true`는 AWS의 Route 53 Resolver 기반 DNS 해석과 관련된 설정이고, ECS Service Connect는 컨테이너 내부의 `/etc/hosts`를 참조하기 때문에 이 둘은 서로 독립적입니다. 다만 어차피 실무에서는 대부분 `"EnableDnsSupport": true`로 설정하기 때문에 크게 신경 쓸 점은 아닙니다.

이전에 살펴보았던 DNS 네임스페이스 리소스인 `"NetworkRetailStoreDnsNamespace0ED145E4"`는 `"retailstore.local"`이라는 Cloud Map Private DNS Namespace를 생성합니다. 이 네임스페이스는 ECS Service Connect에서 서비스들을 묶는 namespace로 사용될 수 있습니다.

```json
  "NetworkRetailStoreDnsNamespace0ED145E4": {
   "Type": "AWS::ServiceDiscovery::PrivateDnsNamespace",
   "Properties": {
    "Description": "Service discovery namespace",
    "Name": "retailstore.local",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Version",
      "Value": "1.0.4"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "Vpc": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

세부 매커니즘을 알아보면, 이 Private DNS Namespace 리소스가 생성되고 VPC에 매핑되면(VPC 내부의 리소스용 DNS Namespace로 사용되면), AWS는 백그라운드에서 Route 53 Private Hosted Zone을 자동으로 생성합니다. 하지만 이 Private DNS Namespace를 ECS Service Connect 용도로 사용할 경우, 애플리케이션 트래픽은 Route 53 Resolver를 거쳐 DNS 레코드를 직접 조회하는 방식으로 전달되지 않습니다. 

ECS Service Connect는 Private DNS Namespace 안에 Service Connect endpoint(예: `http://backend.retailstore.local:8080`)를 생성하고, 컨테이너 내부의 `/etc/hosts` 기반 이름 해석을 통해(이미 `/etc/hosts` 파일 안에 매핑된 DNS 레코드 존재함) 해당 이름이 Service Connect proxy, 즉 Envoy 기반 프록시로 연결되도록 구성합니다.
