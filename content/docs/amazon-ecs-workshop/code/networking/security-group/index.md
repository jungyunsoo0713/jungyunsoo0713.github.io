---
title: "03. 보안 그룹 (Security Groups)"
weight: 3
---
> **작성일:** 2026-04-25 | **수정일:** 2026-04-25
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

Orders 서비스를 위한 보안 그룹을 생성합니다.

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

Orders 서비스에서 외부로 나가는 트래픽에 관한 설정입니다. 여기서 `"CidrIp": "0.0.0.0/0"`은 목적지가 어떤 IPv4 주소여도 된다는 뜻이고, `"IpProtocol": "-1"`은 프로토콜을 제한하지 않겠다는 뜻입니다. 따라서 Orders 서비스는 이 보안 그룹을 기준으로 외부로 나가는 모든 트래픽을 보낼 수 있습니다.

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

`"SecurityGroupIngress"`는 Orders 서비스로 들어오는 트래픽에 관한 설정입니다. 이 설정은 모든 IPv4 주소 범위에서 Orders 서비스의 `8080`번 포트로 들어오는 `TCP` 트래픽을 허용합니다.

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

Orders 서비스가 사용하는 RDS(Aurora PostgreSQL) 클러스터를 위한 보안 그룹을 생성합니다.

---

```json
    "SecurityGroupEgress": [
     {
      "CidrIp": "0.0.0.0/0",
      "Description": "Allow all outbound traffic by default",
      "IpProtocol": "-1"
     }
```

Orders RDS 클러스터에서 외부로 나가는 트래픽에 관한 설정입니다. 이 설정은 Orders RDS 클러스터에서 외부로 나가는 모든 트래픽을 허용합니다.

---

```json
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
```

Orders RDS 클러스터로 들어오는 트래픽에 관한 설정입니다. Orders RDS 클러스터는 `5432`번 포트로 들어오는 `TCP` 트래픽을 허용합니다. `5432`는 PostgreSQL의 기본 포트 번호입니다. 

`"SourceSecurityGroupId"`는 인바운드 트래픽을 허용할 출발지를 IP 주소가 아니라 보안 그룹으로 지정하는 설정입니다. 즉 Orders RDS 클러스터는 Orders 서비스의 보안 그룹을 가진 ECS 태스크에서 오는 트래픽만 받도록 허용합니다. 이는 Orders RDS 클러스터가 Orders 서비스와 직접 통신하도록 구성되어 있기 때문입니다. 

참고로 프라이빗 서브넷의 리소스가 인터넷 방향으로 나가려면 일반적으로 NAT Gateway를 사용할 수 있습니다. 하지만 RDS의 패치와 유지보수는 AWS가 관리형 서비스 영역에서 처리하므로, 이를 RDS 인스턴스의 일반적인 아웃바운드 트래픽으로 설명하는 것은 적절하지 않습니다.

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

Checkout 서비스의 보안 그룹을 생성합니다. 이 보안 그룹은 Orders 서비스의 보안 그룹인 `"NetworkOrdersSecurityGroupA7D5A931"`과 거의 동일한 역할을 합니다.

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

Checkout 서비스가 사용하는 Redis(ElastiCache) 클러스터를 위한 보안 그룹을 생성합니다.

---

이 보안 그룹은 `"NetworkOrdersRDSSecurityGroupD378DF87"`와 거의 같은 구조를 가지고 있습니다. 둘 다 데이터 저장소 역할을 하는 리소스를 위한 보안 그룹이며, 특정 서비스의 보안 그룹에서 들어오는 트래픽만 허용하도록 구성되어 있습니다. 

다만 `"NetworkOrdersRDSSecurityGroupD378DF87"`는 Orders 서비스에서 오는 PostgreSQL 트래픽을 `5432`번 포트로 허용하고, `"NetworkRedisSecurityGroupF069D970"`는 Checkout 서비스에서 오는 Redis 트래픽을 `6379`번 포트로 허용합니다.

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

Catalog 서비스의 보안 그룹을 생성합니다. 이 보안 그룹은 Orders 서비스의 보안 그룹인 `"NetworkOrdersSecurityGroupA7D5A931"`과 거의 동일한 역할을 합니다.

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

Catalog 서비스가 사용하는 RDS(Aurora MySQL) 클러스터를 위한 보안 그룹을 생성합니다.

---

이 보안 그룹은 `"NetworkOrdersRDSSecurityGroupD378DF87"`와 거의 같은 구조를 가지고 있습니다. 둘 다 애플리케이션 서비스가 사용하는 RDS 클러스터를 위한 보안 그룹이며, 각 서비스의 보안 그룹에서 들어오는 데이터베이스 트래픽만 허용하도록 구성되어 있습니다. 

다만 `"NetworkOrdersRDSSecurityGroupD378DF87"`는 Orders 서비스에서 오는 PostgreSQL 트래픽을 `5432`번 포트로 허용하고, `"NetworkCatalogRDSSecurityGroupFFE0C2BE"`는 Catalog 서비스에서 오는 MySQL 트래픽을 `3306`번 포트로 허용합니다.

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

Cart 서비스의 보안 그룹을 생성합니다. 이 보안 그룹은 Orders 서비스의 보안 그룹인 `"NetworkOrdersSecurityGroupA7D5A931"`과 거의 동일한 역할을 합니다.

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

GuardDuty VPC Endpoint를 위한 Security Group을 생성합니다.

---

```json
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
```

이 설정은 VPC2의 CIDR 대역에서 이 보안 그룹이 붙은 VPC Endpoint로 들어오는 `TCP` `443` 포트 트래픽을 허용하는 인바운드 규칙입니다. `"Description"`은 그 규칙을 사람이 읽기 쉽게 설명하기 위해 `from <VPC2 CIDR>:443` 형태의 문자열을 만들어 붙이는 부분입니다.

---

```json
      "FromPort": 443,
      "IpProtocol": "tcp",
      "ToPort": 443
```

443 포트를 사용하는 이유는 GuardDuty Interface VPC Endpoint가 AWS 서비스 API와 `HTTPS`로 통신하기 때문입니다. VPC2 내부 리소스가 GuardDuty 서비스에 프라이빗하게 접근할 때, 요청은 VPC Endpoint의 ENI로 전달되고 이때 `TCP` `443` 포트를 사용합니다. 따라서 이 보안 그룹은 VPC2 내부 CIDR에서 들어오는 `HTTPS` 트래픽을 허용하도록 설정되어 있습니다.
