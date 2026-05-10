---
title: "ECS Service Connect"
weight: 1
---
> **작성일:** 2026-05-10 | **수정일:** 2026-05-10

ECS Service Connect는 ECS 서비스 간 통신에 사용되는 기능입니다. 이 기능은 ECS 서비스를 긴 엔드포인트나 복잡한 주소 대신, 짧은 DNS 이름이나 별칭(Alias)으로 접근할 수 있게 해줍니다. 이 DNS 이름이나 별칭은 ECS 클러스터 내부에서 사용될 수 있고, 네트워크 연결과 관련 설정이 되어 있다면 ECS 클러스터 간, VPC 간, 혹은 같은 AWS Region 안의 AWS 계정 간에도 사용될 수 있습니다.

>VPC의 설정인` "EnableDnsSupport": true`와 ECS Service Connect 활성화 여부는 관계없습니다.` "EnableDnsSupport": true`는 VPC의 DNS 해석 기능을 활성화하는 것이고, ECS Service Connect는 태스크의 컨테이너 내부의` /etc/hosts`를 참조하는 것이기 때문입니다.

---

이전에 생성한 Private DNS Namespace 리소스와 ECS Service Connect가 연관성이 있습니다. 

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

ECS에서 서비스 검색 방식에는 ECS Service Discovery 방식과 ECS Service Connect 방식이 존재합니다.

ECS Service Discovery 방식에서는, Private DNS Namespace 리소스 생성 시 자동으로 생성된 Route 53 Private Hosted Zone이 Private IP 주소와 DNS 이름을 매핑한 DNS 레코드 정보를 저장하고 관리합니다. VPC의 Route 53 Resolver는 이 Route 53 Private Hosted Zone의 DNS 레코드 정보를 참조해 DNS 해석 기능을 제공합니다.

ECS Service Connect 방식에서는, Private DNS Namespace 리소스 생성 시 자동으로 만들어지는 Route 53 Private Hosted Zone을 사용하지 않으며, 해당 Hosted Zone에 DNS 레코드를 생성하지도 않습니다. 즉, ECS Service Connect 때문에 Route 53 Private Hosted Zone의 DNS 레코드 정보가 바뀌지는 않습니다.

ECS Service Connect 방식에서는, 애플리케이션이 목적지 엔드포인트를 호출하면 컨테이너 내부의 `/etc/hosts`를 참조하여 트래픽이 로컬 Service Connect 프록시(Envoy)로 전달됩니다. 이후 프록시는 Amazon ECS가 AWS Cloud Map을 기반으로 생성하고 관리하는 Service Connect endpoint 정보를 바탕으로 트래픽을 로드밸런싱하여 목적지 서비스로 전달합니다. Service Connect endpoint 정보는 ECS 서비스에 Service Connect 설정을 넣고 배포할 때 생성됩니다.



