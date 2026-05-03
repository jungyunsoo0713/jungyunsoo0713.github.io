---
title: "04. 보안 그룹 (Security Groups)"
weight: 4
---
> **작성일:** 2026-05-03 | **수정일:** 2026-05-03
```json
{
  "BasicALBSecurityGroupF9D92961": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for the UI Application Load Balancer",
    "GroupName": "retail-store-ecs-alb",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTP inbound traffic",
      "FromPort": 80,
      "IpProtocol": "tcp",
      "ToPort": 80
     },
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTP inbound traffic for managed instance",
      "FromPort": 8090,
      "IpProtocol": "tcp",
      "ToPort": 8090
     },
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTP for Blue Green test traffic",
      "FromPort": 8080,
      "IpProtocol": "tcp",
      "ToPort": 8080
     },
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTPS inbound traffic",
      "FromPort": 443,
      "IpProtocol": "tcp",
      "ToPort": 443
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   },
   "Metadata": {
    "cdk_nag": {
     "rules_to_suppress": [
      {
       "reason": "This ALB need to be publicly accessible",
       "id": "Workshop-EC2RestrictedInbound"
      }
     ]
    }
   }
  },
  "BasicUISecurityGroup41F5FEAF": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS UI Task",
    "GroupName": "retail-store-ecs-ui-task",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow inbound 8080 traffic",
      "FromPort": 8080,
      "IpProtocol": "tcp",
      "ToPort": 8080
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  }
}
```

---

## 1. `"BasicALBSecurityGroupF9D92961"`

```json
  "BasicALBSecurityGroupF9D92961": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for the UI Application Load Balancer",
    "GroupName": "retail-store-ecs-alb",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTP inbound traffic",
      "FromPort": 80,
      "IpProtocol": "tcp",
      "ToPort": 80
     },
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTP inbound traffic for managed instance",
      "FromPort": 8090,
      "IpProtocol": "tcp",
      "ToPort": 8090
     },
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTP for Blue Green test traffic",
      "FromPort": 8080,
      "IpProtocol": "tcp",
      "ToPort": 8080
     },
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow HTTPS inbound traffic",
      "FromPort": 443,
      "IpProtocol": "tcp",
      "ToPort": 443
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   },
   "Metadata": {
    "cdk_nag": {
     "rules_to_suppress": [
      {
       "reason": "This ALB need to be publicly accessible",
       "id": "Workshop-EC2RestrictedInbound"
      }
     ]
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Application Load Balancer(ALB)에 적용되는 보안 그룹입니다. 보안 그룹은 Task의 ENI에 붙습니다.

---

`"GroupDescription"`은 이 보안 그룹에 대한 설명입니다. UI ALB에 대한 보안 그룹이라는 것을 알려줍니다.

---

`"GroupName"`은 이 보안 그룹의 이름입니다.

---

`"SecurityGroupEgress"`는 보안 그룹이 적용된 리소스에서 외부로 나가는 트래픽을 제어하는 설정입니다.

`"CidrIp": "0.0.0.0/0"`에서 `0.0.0.0/0`은 트래픽의 목적지 IP 주소를 제한하지 않겠다는 의미입니다. `"IpProtocol": "-1"`은 모든 IP 프로토콜을 의미합니다. 따라서 이 조합은 모든 아웃바운드 트래픽을 허용한다는 것을 의미합니다.

---

`"SecurityGroupIngress"`는 보안 그룹이 적용된 리소스 내부로 들어오는 트래픽을 제어하는 설정입니다. 총 4가지 설정이 존재합니다. 각 설정을 보면 모두 `"CidrIp": "0.0.0.0/0"` 옵션을 가지고 있습니다. 이는 들어오는 트래픽의 출발지 IP 주소를 제한하지 않겠다는 의미입니다. 즉, 포트의 프로토콜 조건만 맞다면, 어떤 IPv4 주소에서 오는 트래픽이든 모두 허용될 수 있습니다. 

위 4가지 설정은 각기 다른 포트 범위를 가지고 있습니다. 각각 `80`, `8090`, `8080`, `443`이고, 허용하는 트래픽의 프로토콜은 `TCP`입니다. `HTTP`, `HTTPS`는 `TCP` 프로토콜 위에서 동작하는 프로토콜이고, 이 ALB에 연결되는 컨테이너는 웹 애플리케이션이기 때문에 사실상 `HTTP(S)`를 포함한 웹 기반 트래픽을 위한 설정이라고 볼 수 있습니다.

`8080` 포트로 오는 트래픽을 이 ALB가 받는 이유는 블루/그린 배포 과정에서 새 컨테이너(그린)에 이 트래픽을 보내기 때문입니다. 새 컨테이너를 실제로 사용하기 전에 테스트 트래픽을 보내 정상적으로 작동하는지 확인하기 위해서입니다.

---

`"VpcId"` 필드가 있는 것을 보아, ALB에 연결되는 보안 그룹에는 VPC ID가 필요함을 알 수 있습니다. 이는 이 보안 그룹이 해당 VPC 안에서 생성되기 때문입니다.

---

`"Metadata"`는 이 리소스에 대한 추가 정보가 있는 필드입니다.

---

`"cdk_nag"`은 AWS CDK 코드가 보안 모범 사례(Best Practice)를 잘 지키고 있는지 검사하는 도구입니다. `"rules_to_suppress"`에 지정된 것은 해당 경고를 무시하겠다는 것을 의미합니다. 여기서는 `"Workshop-EC2RestrictedInbound"` 경고를 무시하며, 그 이유는 이 ALB가 퍼블릭 접근을 허용해야 하기 때문입니다. 

---

## 2. `"BasicUISecurityGroup41F5FEAF"`

```json
  "BasicUISecurityGroup41F5FEAF": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS UI Task",
    "GroupName": "retail-store-ecs-ui-task",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow inbound 8080 traffic",
      "FromPort": 8080,
      "IpProtocol": "tcp",
      "ToPort": 8080
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  }
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 UI Service에서 실행되는 UI Task에 적용되는 보안 그룹입니다.

---

`"SecurityGroupEgress"`는 ALB에 적용된 보안 그룹과 같은 설정을 가지고 있습니다. 즉 모든 아웃바운드 트래픽을 허용합니다.

---

`"SecurityGroupIngress"`를 보면 ALB에 적용된 보안 그룹과 다르게 UI Task는 `8080` 포트로 오는 `TCP`요청만 받습니다. 일반적으로 ALB가 클라이언트로부터 `80` 또는 `443` 포트로 요청을 받고, `443` 요청의 경우 TLS를 종료한 뒤 UI Task의 `8080` 포트로 새 요청을 전달하기 때문입니다.

---

이 보안 그룹은 프라이빗 서브넷에 배치된 UI Task에 적용되기 때문에, 인터넷에서 직접 접근하는 대상이 아닙니다. 따라서 ALB에 적용된 보안 그룹처럼 `"rules_to_suppress"` 필드를 사용해서 예외 처리를 할 필요는 없는 것으로 볼 수 있습니다.
