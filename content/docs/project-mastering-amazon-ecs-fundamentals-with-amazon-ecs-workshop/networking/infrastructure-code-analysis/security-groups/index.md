---
title: "03. 보안 그룹 (Security Groups)"
weight: 3
---
> 	**작성일:** 2026-05-05 | **수정일:** 2026-05-05
```json
{
  "NetworkOrdersSecurityGroupA7D5A931": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Orders Task",
    "GroupName": "retail-store-ecs-orders-task",
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkOrdersRDSSecurityGroupD378DF87": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security group for Orders RDS instance",
    "GroupName": "retail-store-ecs-orders-rds",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "Description": "Allow PostgreSQL access",
      "FromPort": 5432,
      "IpProtocol": "tcp",
      "SourceSecurityGroupId": {
       "Fn::GetAtt": [
        "NetworkOrdersSecurityGroupA7D5A931",
        "GroupId"
       ]
      },
      "ToPort": 5432
     }
    ],
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkCheckoutSecurityGroupFBC9C26C": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Checkout Task",
    "GroupName": "retail-store-ecs-checkout-task",
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkRedisSecurityGroupF069D970": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security group for Redis cluster",
    "GroupName": "retail-store-ecs-redis",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "Description": "Allow Redis access",
      "FromPort": 6379,
      "IpProtocol": "tcp",
      "SourceSecurityGroupId": {
       "Fn::GetAtt": [
        "NetworkCheckoutSecurityGroupFBC9C26C",
        "GroupId"
       ]
      },
      "ToPort": 6379
     }
    ],
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkCatalogSecurityGroupEFFC643F": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Catalog Task",
    "GroupName": "retail-store-ecs-catalog-task",
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkCatalogRDSSecurityGroupFFE0C2BE": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security group for Catalog RDS instance",
    "GroupName": "retail-store-ecs-catalog-rds",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "Description": "Allow MySQL access",
      "FromPort": 3306,
      "IpProtocol": "tcp",
      "SourceSecurityGroupId": {
       "Fn::GetAtt": [
        "NetworkCatalogSecurityGroupEFFC643F",
        "GroupId"
       ]
      },
      "ToPort": 3306
     }
    ],
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkCartSecurityGroupVPC27CCFCFC6": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Cart Task VPC2",
    "GroupName": "retail-store-ecs-carts",
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkGuardDutyEndpointSecurityGroupVPCLatticeEA4520D9": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for GuardDuty VPC Endpoint",
    "GroupName": "retail-store-ecs-guardduty-vpclattice",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "CidrIp": {
       "Fn::GetAtt": [
        "NetworkRetailStoreVPC2A069619D",
        "CidrBlock"
       ]
      },
      "Description": {
       "Fn::Join": [
        "",
        [
         "from ",
         {
          "Fn::GetAtt": [
           "NetworkRetailStoreVPC2A069619D",
           "CidrBlock"
          ]
         },
         ":443"
        ]
       ]
      },
      "FromPort": 443,
      "IpProtocol": "tcp",
      "ToPort": 443
     }
    ],
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  }
}
```

---

## 1. `"NetworkOrdersSecurityGroupA7D5A931"`

```json
  "NetworkOrdersSecurityGroupA7D5A931": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Orders Task",
    "GroupName": "retail-store-ecs-orders-task",
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Orders 서비스의 Task에 적용되는 보안 그룹입니다.

---

Orders 서비스의 Task에 적용되는 보안 그룹은 `8080` 포트로 들어오는 `TCP` 트래픽만 허용합니다. 이는 Orders 서비스의 Task가 `8080` 포트를 사용하기 때문입니다.

클라이언트가 ALB에 `80` 이나 `443` 포트로 요청을 하면, ALB는 그 요청 트래픽의 일부 정보를 추가하거나 수정한 후(`443` 포트로 온 요청의 경우 TLS를 종료함), `8080`번 포트를 목적지로 하는 별도의 트래픽을 생성해 타깃 그룹의 타깃(Order 서비스의 태스크 등)으로 전달합니다.

---

## 2. `"NetworkOrdersRDSSecurityGroupD378DF87"`

```json
  "NetworkOrdersRDSSecurityGroupD378DF87": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security group for Orders RDS instance",
    "GroupName": "retail-store-ecs-orders-rds",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "Description": "Allow PostgreSQL access",
      "FromPort": 5432,
      "IpProtocol": "tcp",
      "SourceSecurityGroupId": {
       "Fn::GetAtt": [
        "NetworkOrdersSecurityGroupA7D5A931",
        "GroupId"
       ]
      },
      "ToPort": 5432
     }
    ],
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Orders 서비스의 DB 인스턴스에 적용되는 보안 그룹입니다.

---

`"SecurityGroupIngress"`인바운드 규칙을 보면, `5432` 포트로 오는 `TCP` 트래픽을 받는다는 것을 알 수 있습니다. 

`"SourceSecurityGroupId"`는 이 인바운드 규칙에서 어떤 보안 그룹을 가진 리소스의 트래픽을 허용할지 정합니다. 여기서는 앞서 생성한 Orders 서비스의 Task에서 오는 트래픽만 허용합니다. `"NetworkOrdersSecurityGroupA7D5A931"` 보안 그룹이 Orders 서비스의 Task에 적용되었기 때문입니다.

---

## 3. `"NetworkCheckoutSecurityGroupFBC9C26C"`

```json
  "NetworkCheckoutSecurityGroupFBC9C26C": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Checkout Task",
    "GroupName": "retail-store-ecs-checkout-task",
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Checkout 서비스의 Task에 적용되는 보안 그룹입니다.

---

## 4. `"NetworkRedisSecurityGroupF069D970"`

```json
  "NetworkRedisSecurityGroupF069D970": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security group for Redis cluster",
    "GroupName": "retail-store-ecs-redis",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "Description": "Allow Redis access",
      "FromPort": 6379,
      "IpProtocol": "tcp",
      "SourceSecurityGroupId": {
       "Fn::GetAtt": [
        "NetworkCheckoutSecurityGroupFBC9C26C",
        "GroupId"
       ]
      },
      "ToPort": 6379
     }
    ],
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Redis 클러스터에 적용되는 보안 그룹입니다. 참고로 Redis 캐시는 서버리스로 생성되었기 때문에, Redis 클러스터는 Checkout 서비스 내부에 위치한 리소스가 아닙니다. 

---

## 5. `"NetworkCatalogSecurityGroupEFFC643F"`

```json
  "NetworkCatalogSecurityGroupEFFC643F": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Catalog Task",
    "GroupName": "retail-store-ecs-catalog-task",
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Catalog 서비스의 Task에 적용되는 보안 그룹입니다.

---

## 6. `"NetworkCatalogRDSSecurityGroupFFE0C2BE"`

```json
  "NetworkCatalogRDSSecurityGroupFFE0C2BE": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security group for Catalog RDS instance",
    "GroupName": "retail-store-ecs-catalog-rds",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "Description": "Allow MySQL access",
      "FromPort": 3306,
      "IpProtocol": "tcp",
      "SourceSecurityGroupId": {
       "Fn::GetAtt": [
        "NetworkCatalogSecurityGroupEFFC643F",
        "GroupId"
       ]
      },
      "ToPort": 3306
     }
    ],
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
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Catalog 서비스의 DB 인스턴스에 적용되는 보안 그룹입니다.

---

## 7. `"NetworkCartSecurityGroupVPC27CCFCFC6"`

```json
  "NetworkCartSecurityGroupVPC27CCFCFC6": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for ECS Cart Task VPC2",
    "GroupName": "retail-store-ecs-carts",
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 Carts 서비스의 Task에 적용되는 보안 그룹입니다.

---

## 8. `"NetworkGuardDutyEndpointSecurityGroupVPCLatticeEA4520D9"`

```json
  "NetworkGuardDutyEndpointSecurityGroupVPCLatticeEA4520D9": {
   "Type": "AWS::EC2::SecurityGroup",
   "Properties": {
    "GroupDescription": "Security Group for GuardDuty VPC Endpoint",
    "GroupName": "retail-store-ecs-guardduty-vpclattice",
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
    ],
    "SecurityGroupIngress": [
     {
      "CidrIp": {
       "Fn::GetAtt": [
        "NetworkRetailStoreVPC2A069619D",
        "CidrBlock"
       ]
      },
      "Description": {
       "Fn::Join": [
        "",
        [
         "from ",
         {
          "Fn::GetAtt": [
           "NetworkRetailStoreVPC2A069619D",
           "CidrBlock"
          ]
         },
         ":443"
        ]
       ]
      },
      "FromPort": 443,
      "IpProtocol": "tcp",
      "ToPort": 443
     }
    ],
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  }
```

보안 그룹을 생성하는 리소스입니다. 이 보안 그룹은 VPC2 내부 리소스가 GuardDuty API에 접근하기 위해 사용하는 Interface VPC Endpoint에 적용됩니다. GuardDuty는 VPC 외부에 있는 AWS 관리형 서비스입니다. VPC 내부의 리소스들은 인터넷을 거치지 않고 GuardDuty API와 통신하기 위해 VPC 내부에 생성된 VPC Endpoint를 이용합니다. 

GuardDuty는 AWS에서 발생하는 의심스러운 보안 이벤트를 탐지하는 서비스입니다. 로그와 이벤트를 분석해 의심스러운 활동을 찾아냅니다.

참고로 GuardDuty가 아니라 GuardDuty API와 통신한다고 표현하는 이유는, VPC 내부의 리소스가 VPC Endpoint를 통해 GuardDuty API를 호출하는 요청을 보내기 때문입니다.

---

`"SecurityGroupIngress"` 인바운드 규칙을 보면, `"CidrIp"`에 VPC2의 `"CidrBlock"` 값이 적용됨을 알 수 있습니다. 이 값은 `"192.168.0.0/16"`입니다. 이러한 설정의 이유는 GuardDuty용 VPC Endpoint가 VPC2 내부 리소스에서 오는 트래픽만 받도록 제한하기 위함입니다.

---

`"FromPort": 443`, `"IpProtocol": "tcp"`, `"ToPort": 443`을 보면, GuardDuty VPC Endpoint는 `443` 포트로 들어오는 `TCP` 요청을 받는다는 것을 알 수 있습니다. 이는 GuardDuty API를 호출하는 요청이 기본적으로 `HTTPS` 요청이기 때문입니다.
