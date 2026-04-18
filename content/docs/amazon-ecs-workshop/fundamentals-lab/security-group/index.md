---
title: "04. 보안 그룹 (Security Groups)"
weight: 4
---
> **작성일:** 2026-04-18 | **수정일:** 2026-04-18
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

ALB(Application Load Balancer)에 대한 보안 그룹(Security Group)을 생성합니다. 일반적으로는 리소스에 적용할 네트워크 접근 제어 정책을 먼저 정해야 하므로, 보안 그룹을 먼저 정의한 뒤 이를 리소스에 연결하는 방식으로 구성합니다.

---

`"GroupDescription": "Security Group for the UI Application Load Balancer"`는 UI 애플리케이션용 로드 밸런서에 연결되는 보안 그룹이라는 뜻입니다. 사용자가 웹 애플리케이션의 UI(프론트엔드)로 요청을 보내면, 이 로드 밸런서가 해당 요청을 받아 타깃 그룹에 등록된 프라이빗 서브넷의 인스턴스나 태스크로 분산합니다.

---

```json
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
```

`"SecurityGroupEgress"`는 이 보안 그룹이 붙은 리소스가 외부로 내보내는 트래픽에 대한 규칙입니다. `"CidrIp": "0.0.0.0/0"`은 모든 목적지 IP 주소 대역을 의미하고, `"IpProtocol": "-1"`은 TCP, UDP, ICMP 등을 포함한 모든 프로토콜을 의미합니다. 즉 이 설정은 해당 리소스가 어떤 목적지로든 모든 프로토콜을 사용해 아웃바운드 트래픽을 보낼 수 있도록 허용합니다.

실무에서는 아웃바운드 트래픽도 꼭 필요한 목적지 IP 주소와 포트만 허용하고, 이를 지속적으로 점검하고 검증합니다.

```json
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
```

`"SecurityGroupIngress"`는 이 보안 그룹이 붙은 리소스가 외부로부터 받는 트래픽에 대한 규칙입니다. `"CidrIp": "0.0.0.0/0"`은 모든 IP 주소 대역을 의미하며, `"FromPort"` 값은 허용할 포트 범위의 시작 번호를, `"ToPort"` 값은 허용할 포트 범위의 끝 번호를 의미합니다. 이 설정은 모든 IP 주소 대역에서 들어오는 TCP 트래픽 중 `80`, `8090`, `8080`, `443` 포트에 대한 접근을 허용합니다.

---

```json
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

메타데이터를 의미합니다. 이 메타데이터는 `cdk-nag` 경고를 처리하기 위한 설정입니다. `cdk-nag`은 AWS CDK 코드에 대해 보안 및 베스트 프랙티스 검사를 수행하는 도구입니다. `"rules_to_suppress"`는 특정 검사 규칙에서 발생하는 경고를 무시하도록 설정하는 항목입니다. 여기서는 ALB가 외부에서 접근 가능한 진입점이어야 하므로, 공개 인바운드 트래픽 허용에 대한 `cdk-nag` 경고를 무시하도록 설정했습니다. `"id": "Workshop-EC2RestrictedInbound"`는 이러한 공개 인바운드 허용과 관련된 검사 규칙을 의미합니다. 

참고로 `"id": "Workshop-EC2RestrictedInbound"`는 이 규칙이 EC2에 관련된 규칙이라는 뜻은 아닙니다. 단지 `cdk-nag`에서 사용하는 검사 규칙의 이름일 뿐이며, 여기서는 공개 인바운드 트래픽 허용과 관련된 경고를 식별하는 용도로 사용됩니다.

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

`"GroupDescription": "Security Group for ECS UI Task"`는 이 보안 그룹이 ECS UI Task용 보안 그룹임을 설명하는 값입니다.

---

```json
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
```

앞의 로드 밸런서에 연결된 보안 그룹과 마찬가지로, 이 보안 그룹도 프로토콜과 관계없이 모든 아웃바운드 트래픽을 허용합니다. 

---

```json
	"SecurityGroupIngress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow inbound 8080 traffic",
      "FromPort": 8080,
      "IpProtocol": "tcp",
      "ToPort": 8080
     }
    ],
```

모든 IP 주소 대역에서 들어오는 TCP `8080` 포트 트래픽을 허용합니다.

로드 밸런서에 연결된 보안 그룹처럼 여러 포트를 허용하지 않고 오직 `8080` 포트 트래픽만 허용하는 이유는, 이 보안 그룹이 연결된 ECS UI Task가 `8080` 포트에서만 애플리케이션 요청을 받기 때문입니다. 여기서 `8080` 포트는 이 애플리케이션이 HTTP 요청을 수신하도록 열어둔 포트입니다.

참고로 사용자는 HTTPS(`443` 포트)로 ALB에 요청을 보내고, ALB는 이 HTTPS 연결을 종료(TLS 종료)한 뒤 백엔드인 ECS UI Task에 HTTP로 요청을 전달합니다.

---

이 설정 역시 `0.0.0.0/0`에서 `8080` 포트 접근을 허용하므로 공개 인바운드 트래픽에 해당합니다. 따라서 보안 관점에서는 ALB처럼 `cdk-nag` 경고 대상이 될 수 있습니다. 다만 이 예제에서는 실습 편의를 위해 단순화한 설정을 사용한 것으로 보입니다.
