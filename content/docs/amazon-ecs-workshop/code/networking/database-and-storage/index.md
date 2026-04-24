---
title: 02. 데이터베이스 및 스토리지 (RDS, Redis, DynamoDB)
weight: 2
---
> **작성일:** 2026-04-24 | **수정일:** 2026-04-24
```json
{
  "NetworkOrdersClusterSubnets2E257B7A": {
   "Type": "AWS::RDS::DBSubnetGroup",
   "Properties": {
    "DBSubnetGroupDescription": "Subnets for OrdersCluster database",
    "SubnetIds": [
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173"
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
  "NetworkOrdersCluster095F1433": {
   "Type": "AWS::RDS::DBCluster",
   "Properties": {
    "CopyTagsToSnapshot": true,
    "DBClusterIdentifier": "retail-store-ecs-orders",
    "DBClusterParameterGroupName": "default.aurora-postgresql17",
    "DBSubnetGroupName": {
     "Ref": "NetworkOrdersClusterSubnets2E257B7A"
    },
    "DatabaseName": "orders",
    "DeletionProtection": false,
    "Engine": "aurora-postgresql",
    "EngineVersion": "17.4",
    "MasterUserPassword": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkDBCredentialsSecretADE5ABB3"
       },
       ":SecretString:password::}}"
      ]
     ]
    },
    "MasterUsername": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkDBCredentialsSecretADE5ABB3"
       },
       ":SecretString:username::}}"
      ]
     ]
    },
    "Port": 5432,
    "StorageEncrypted": true,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
    "VpcSecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkOrdersRDSSecurityGroupD378DF87",
       "GroupId"
      ]
     }
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
  "NetworkOrdersClusterwriterD79E7658": {
   "Type": "AWS::RDS::DBInstance",
   "Properties": {
    "DBClusterIdentifier": {
     "Ref": "NetworkOrdersCluster095F1433"
    },
    "DBInstanceClass": "db.t3.medium",
    "Engine": "aurora-postgresql",
    "PromotionTier": 0,
    "PubliclyAccessible": false,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
   },
   "DependsOn": [
    "BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2",
    "BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100",
    "BasicRetailStoreVPCPrivateSubnet2DefaultRoute6CB95A04",
    "BasicRetailStoreVPCPrivateSubnet2RouteTableAssociationA0687748",
    "BasicRetailStoreVPCPrivateSubnet3DefaultRoute11F29836",
    "BasicRetailStoreVPCPrivateSubnet3RouteTableAssociationF2AF7FA5"
   ],
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
  "NetworkCatalogClusterSubnets5A1720E9": {
   "Type": "AWS::RDS::DBSubnetGroup",
   "Properties": {
    "DBSubnetGroupDescription": "Subnets for CatalogCluster database",
    "SubnetIds": [
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173"
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
  "NetworkCatalogClusterB0717C37": {
   "Type": "AWS::RDS::DBCluster",
   "Properties": {
    "CopyTagsToSnapshot": true,
    "DBClusterIdentifier": "retail-store-ecs-catalog",
    "DBClusterParameterGroupName": "default.aurora-mysql8.0",
    "DBSubnetGroupName": {
     "Ref": "NetworkCatalogClusterSubnets5A1720E9"
    },
    "DatabaseName": "catalog",
    "DeletionProtection": false,
    "Engine": "aurora-mysql",
    "EngineVersion": "8.0.mysql_aurora.3.09.0",
    "MasterUserPassword": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
       },
       ":SecretString:password::}}"
      ]
     ]
    },
    "MasterUsername": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
       },
       ":SecretString:username::}}"
      ]
     ]
    },
    "StorageEncrypted": true,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
    "VpcSecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkCatalogRDSSecurityGroupFFE0C2BE",
       "GroupId"
      ]
     }
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
  "NetworkCatalogClusterwriter083FD161": {
   "Type": "AWS::RDS::DBInstance",
   "Properties": {
    "DBClusterIdentifier": {
     "Ref": "NetworkCatalogClusterB0717C37"
    },
    "DBInstanceClass": "db.t3.medium",
    "Engine": "aurora-mysql",
    "PromotionTier": 0,
    "PubliclyAccessible": false,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
   },
   "DependsOn": [
    "BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2",
    "BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100",
    "BasicRetailStoreVPCPrivateSubnet2DefaultRoute6CB95A04",
    "BasicRetailStoreVPCPrivateSubnet2RouteTableAssociationA0687748",
    "BasicRetailStoreVPCPrivateSubnet3DefaultRoute11F29836",
    "BasicRetailStoreVPCPrivateSubnet3RouteTableAssociationF2AF7FA5"
   ],
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
  "NetworkRedisCluster8A158537": {
   "Type": "AWS::ElastiCache::ServerlessCache",
   "Properties": {
    "Description": "Redis cluster for checkout service",
    "Engine": "redis",
    "SecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkRedisSecurityGroupF069D970",
       "GroupId"
      ]
     }
    ],
    "ServerlessCacheName": "retail-store-ecs-redis",
    "SubnetIds": [
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173"
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
    ]
   },
   "DeletionPolicy": "Delete"
  },
  "NetworkCartsTableVPCLattice03AD316C": {
   "Type": "AWS::DynamoDB::Table",
   "Properties": {
    "AttributeDefinitions": [
     {
      "AttributeName": "id",
      "AttributeType": "S"
     },
     {
      "AttributeName": "customerId",
      "AttributeType": "S"
     }
    ],
    "BillingMode": "PAY_PER_REQUEST",
    "GlobalSecondaryIndexes": [
     {
      "IndexName": "idx_global_customerId",
      "KeySchema": [
       {
        "AttributeName": "customerId",
        "KeyType": "HASH"
       }
      ],
      "Projection": {
       "ProjectionType": "ALL"
      }
     }
    ],
    "KeySchema": [
     {
      "AttributeName": "id",
      "KeyType": "HASH"
     }
    ],
    "TableName": "retail-store-ecs-carts-vpclattice",
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
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  }
}
```

---
## 1. `"NetworkOrdersClusterSubnets2E257B7A"`

```json
  "NetworkOrdersClusterSubnets2E257B7A": {
   "Type": "AWS::RDS::DBSubnetGroup",
   "Properties": {
    "DBSubnetGroupDescription": "Subnets for OrdersCluster database",
    "SubnetIds": [
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173"
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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

`"NetworkOrdersClusterSubnets2E257B7A"`는 Orders DB가 사용할 DB Subnet Group을 생성하는 리소스입니다. DB Subnet Group은 RDS/Aurora 데이터베이스가 VPC 안의 어떤 서브넷들에 배치될 수 있는지 정하는 서브넷 묶음입니다. 여기서 Orders DB가 사용할 서브넷은 Fundamentals Lab에서 생성한 프라이빗 서브넷 3개입니다.

---
## 2. `"NetworkOrdersCluster095F1433"`

```json
  "NetworkOrdersCluster095F1433": {
   "Type": "AWS::RDS::DBCluster",
   "Properties": {
    "CopyTagsToSnapshot": true,
    "DBClusterIdentifier": "retail-store-ecs-orders",
    "DBClusterParameterGroupName": "default.aurora-postgresql17",
    "DBSubnetGroupName": {
     "Ref": "NetworkOrdersClusterSubnets2E257B7A"
    },
    "DatabaseName": "orders",
    "DeletionProtection": false,
    "Engine": "aurora-postgresql",
    "EngineVersion": "17.4",
    "MasterUserPassword": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkDBCredentialsSecretADE5ABB3"
       },
       ":SecretString:password::}}"
      ]
     ]
    },
    "MasterUsername": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkDBCredentialsSecretADE5ABB3"
       },
       ":SecretString:username::}}"
      ]
     ]
    },
    "Port": 5432,
    "StorageEncrypted": true,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
    "VpcSecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkOrdersRDSSecurityGroupD378DF87",
       "GroupId"
      ]
     }
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
```

Orders 서비스가 사용할 Aurora PostgreSQL DB 클러스터를 생성합니다. DB 클러스터란 DB를 운영하기 위한 논리적인 묶음입니다. DB 클러스터에는 DB 엔진, 엔진 버전, 포트, 서브넷 그룹, 보안 그룹, 관리자 계정 정보, 암호화 설정 등이 포함됩니다. Aurora의 경우, 클러스터에 DB 인스턴스가 연결되어 실제 애플리케이션의 접속과 쿼리 처리를 담당합니다. 이하 이 글에서 DB라고 표현하는 것은 특별한 언급이 없는 한 DB 클러스터를 의미합니다.

---

`"CopyTagsToSnapshot": true`은 DB 클러스터에 붙어 있는 태그를 DB 스냅샷에도 복사하겠다는 설정입니다. 스냅샷이란 특정 시점의 DB 상태를 저장한 백업본입니다. 

---

`"DBClusterParameterGroupName"`은 DB 클러스터에 적용할 파라미터 그룹 이름을 지정하는 설정입니다. 파라미터 그룹은 DB 엔진의 설정값 묶음입니다. 여기에는 쿼리 실행 관련 설정, 메모리 사용 관련 설정, 로그 관련 설정 등이 포함될 수 있습니다.

---

```json
    "MasterUserPassword": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkDBCredentialsSecretADE5ABB3"
       },
       ":SecretString:password::}}"
      ]
     ]
    },
    "MasterUsername": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkDBCredentialsSecretADE5ABB3"
       },
       ":SecretString:username::}}"
      ]
     ]
    },
```

 DB 관리자 계정 정보를 설정합니다. 여기서는 DB 관리자의 사용자 이름(`MasterUsername"`)과 비밀번호(`"MasterUserPassword"`)를 템플릿에 직접 하드코딩하지 않고, AWS Secrets Manager에 저장된 시크릿 값에서 동적으로 불러옵니다.

---

`"Port": 5432`는 PostgreSQL의 기본 포트 번호입니다.

---

`"StorageEncrypted": true`는 DB 스토리지를 암호화하겠다는 설정입니다. 이 설정을 사용하면 데이터베이스에 저장되는 데이터가 디스크에 암호화된 상태로 저장됩니다. 즉, 저장 시 암호화(encryption at rest)를 활성화하는 설정입니다. 정상적인 경로로 데이터를 읽을 때는 RDS/Aurora가 자동으로 복호화를 처리하므로, 애플리케이션은 별도의 복호화 로직 없이 데이터를 읽을 수 있습니다.

---

`"VpcSecurityGroupIds"`는 DB 클러스터에 적용할 보안 그룹 ID 목록을 의미합니다.

---

`"DeletionPolicy": "Delete"`는 이 DB를 관리하는 CloudFormation 스택이 삭제될 때, 이 DB도 함께 삭제하겠다는 의미입니다.

`"UpdateReplacePolicy": "Delete"`는 스택 업데이트 과정에서 이 DB를 새 리소스로 교체해야 할 때, 기존 DB를 보존하지 않고 삭제하겠다는 의미입니다.

DB에는 중요한 데이터가 저장될 수 있기 때문에 운영 환경에서는 실수로 데이터가 삭제되는 것을 막기 위해 `"Retain"`이나 `"Snapshot"`을 사용하는 경우가 많습니다.

`"Retain"`은 CloudFormation 스택에서 리소스가 제거되더라도 실제 AWS 리소스는 삭제하지 않고 남겨두겠다는 설정입니다. 즉, 스택은 삭제되지만 DB 자체는 AWS 계정 안에 계속 남아 있습니다. 중요한 데이터를 보존해야 할 때 사용할 수 있지만, 리소스가 남아 있으므로 이후 직접 관리하거나 삭제해야 합니다.

`"Snapshot"`은 리소스를 삭제하기 전에 스냅샷을 먼저 생성하겠다는 설정입니다. DB 자체는 삭제되지만, 삭제 직전 상태의 백업본이 스냅샷으로 남기 때문에 나중에 필요하면 해당 스냅샷을 기반으로 DB를 복원할 수 있습니다.

이 코드는 워크숍 실습용 리소스이므로, 실습 후 리소스를 쉽게 정리할 수 있도록 `"Delete"`로 설정한 것으로 보입니다.

---
## 3. `"NetworkOrdersClusterwriterD79E7658"`

```json
  "NetworkOrdersClusterwriterD79E7658": {
   "Type": "AWS::RDS::DBInstance",
   "Properties": {
    "DBClusterIdentifier": {
     "Ref": "NetworkOrdersCluster095F1433"
    },
    "DBInstanceClass": "db.t3.medium",
    "Engine": "aurora-postgresql",
    "PromotionTier": 0,
    "PubliclyAccessible": false,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
   },
   "DependsOn": [
    "BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2",
    "BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100",
    "BasicRetailStoreVPCPrivateSubnet2DefaultRoute6CB95A04",
    "BasicRetailStoreVPCPrivateSubnet2RouteTableAssociationA0687748",
    "BasicRetailStoreVPCPrivateSubnet3DefaultRoute11F29836",
    "BasicRetailStoreVPCPrivateSubnet3RouteTableAssociationF2AF7FA5"
   ],
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
```

`"NetworkOrdersClusterwriterD79E7658"`는 DB 클러스터에 연결되는 writer DB 인스턴스입니다. DB 인스턴스는 역할에 따라 writer 인스턴스와 reader 인스턴스로 구분할 수 있습니다. writer 인스턴스는 `INSERT`, `UPDATE`, `DELETE` 등의 쓰기 요청을 처리하고, reader 인스턴스는 `SELECT` 같은 읽기 요청을 처리합니다. 이 코드에서는 writer 인스턴스를 생성합니다.

---

`"DBInstanceClass": "db.t3.medium"`은 이 DB 인스턴스에 사용할 컴퓨팅 사양을 지정하는 설정입니다. 물리 하드웨어를 직접 선택하는 것은 아니지만, CPU, 메모리, 네트워크 성능 같은 DB 인스턴스의 성능 등급을 정합니다.

---

`"PromotionTier"`는 DB 클러스터에서 장애가 발생해 장애 조치가 수행될 때, 어떤 reader 인스턴스를 먼저 writer 인스턴스로 승격할지 정하는 우선순위입니다. 숫자가 낮을수록 우선순위가 높습니다.

일반적으로 Aurora DB 클러스터는 writer 인스턴스 1개와 reader 인스턴스 0개 이상으로 구성됩니다. DB에서 쓰기 작업은 테이블의 데이터를 변경하는 작업입니다. 여러 인스턴스가 동시에 쓰기 작업을 처리하면 데이터 충돌이나 정합성 문제가 생길 수 있기 때문에, 일반적으로 하나의 writer 인스턴스가 쓰기 요청을 처리합니다. 반면 읽기 작업은 데이터를 조회하는 작업이며, 테이블의 데이터를 변경하지 않습니다. 따라서 여러 reader 인스턴스가 동시에 읽기 요청을 나누어 처리해도 비교적 안전합니다. 

writer 인스턴스에 장애가 발생하고 reader 인스턴스가 존재하는 경우, Aurora는 reader 인스턴스 중 하나를 새로운 writer 인스턴스로 승격하여 쓰기 요청을 처리할 수 있습니다.

다만 이 코드에서는 별도의 reader 인스턴스를 생성하지 않고 writer 인스턴스 하나만 생성하고 있으므로, 여러 reader 인스턴스 사이의 승격 우선순위를 비교하는 상황은 발생하지 않습니다.

---

`"PubliclyAccessible": false`는 이 DB 인스턴스에 Public IP를 부여하지 않음으로써, 인터넷에서 직접 접근할 수 없도록 설정한다는 의미입니다. 일반적으로 DB는 외부 인터넷에 직접 노출하지 않고, ECS 서비스 같은 애플리케이션이 VPC 내부에서 접근하도록 구성합니다.

---

```json
   "DependsOn": [
    "BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2",
    "BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100",
    "BasicRetailStoreVPCPrivateSubnet2DefaultRoute6CB95A04",
    "BasicRetailStoreVPCPrivateSubnet2RouteTableAssociationA0687748",
    "BasicRetailStoreVPCPrivateSubnet3DefaultRoute11F29836",
    "BasicRetailStoreVPCPrivateSubnet3RouteTableAssociationF2AF7FA5"
   ]
```

이 설정은 DB 인스턴스가 생성되기 전에 네트워크 라우팅 구성이 먼저 준비되도록 생성 순서를 명시하는 역할을 합니다.

---
## 4. `"NetworkCatalogClusterSubnets5A1720E9"`

```json
  "NetworkCatalogClusterSubnets5A1720E9": {
   "Type": "AWS::RDS::DBSubnetGroup",
   "Properties": {
    "DBSubnetGroupDescription": "Subnets for CatalogCluster database",
    "SubnetIds": [
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173"
     }
    ],
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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

Catalog DB를 위한 DB Subnet Group을 생성합니다. 앞에서 살펴본 Orders DB용 DB Subnet Group(`"NetworkOrdersClusterSubnets2E257B7A"`)과 구성은 거의 동일하며, 동일한 프라이빗 서브넷 3개를 사용합니다.

---
## 5. `"NetworkCatalogClusterB0717C37"`

```json
  "NetworkCatalogClusterB0717C37": {
   "Type": "AWS::RDS::DBCluster",
   "Properties": {
    "CopyTagsToSnapshot": true,
    "DBClusterIdentifier": "retail-store-ecs-catalog",
    "DBClusterParameterGroupName": "default.aurora-mysql8.0",
    "DBSubnetGroupName": {
     "Ref": "NetworkCatalogClusterSubnets5A1720E9"
    },
    "DatabaseName": "catalog",
    "DeletionProtection": false,
    "Engine": "aurora-mysql",
    "EngineVersion": "8.0.mysql_aurora.3.09.0",
    "MasterUserPassword": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
       },
       ":SecretString:password::}}"
      ]
     ]
    },
    "MasterUsername": {
     "Fn::Join": [
      "",
      [
       "{{resolve:secretsmanager:",
       {
        "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
       },
       ":SecretString:username::}}"
      ]
     ]
    },
    "StorageEncrypted": true,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
    "VpcSecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkCatalogRDSSecurityGroupFFE0C2BE",
       "GroupId"
      ]
     }
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
```

`"NetworkCatalogClusterB0717C37"`는 Catalog 서비스가 사용할 Aurora MySQL DB 클러스터를 생성하는 리소스입니다. 앞에서 살펴본 Orders DB 클러스터와 전체 구성은 거의 동일합니다.

다만 Orders DB는 `"Engine": "aurora-postgresql"`을 사용하는 Aurora PostgreSQL 클러스터이고, Catalog DB는 `"Engine": "aurora-mysql"`을 사용하는 Aurora MySQL 클러스터입니다. 따라서 클러스터 이름, 데이터베이스 이름, 파라미터 그룹, 엔진 버전, Secrets Manager 시크릿, 보안 그룹, DB Subnet Group이 서로 다릅니다.

---
## 6. `"NetworkCatalogClusterwriter083FD161"`

```json
  "NetworkCatalogClusterwriter083FD161": {
   "Type": "AWS::RDS::DBInstance",
   "Properties": {
    "DBClusterIdentifier": {
     "Ref": "NetworkCatalogClusterB0717C37"
    },
    "DBInstanceClass": "db.t3.medium",
    "Engine": "aurora-mysql",
    "PromotionTier": 0,
    "PubliclyAccessible": false,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "environment-name",
      "Value": "retail-store-ecs"
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
   },
   "DependsOn": [
    "BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2",
    "BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100",
    "BasicRetailStoreVPCPrivateSubnet2DefaultRoute6CB95A04",
    "BasicRetailStoreVPCPrivateSubnet2RouteTableAssociationA0687748",
    "BasicRetailStoreVPCPrivateSubnet3DefaultRoute11F29836",
    "BasicRetailStoreVPCPrivateSubnet3RouteTableAssociationF2AF7FA5"
   ],
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  },
```

`"NetworkCatalogClusterwriter083FD161"`는 Catalog DB 클러스터에 연결되는 writer DB 인스턴스를 생성하는 리소스입니다. 앞에서 살펴본 Orders DB의 writer 인스턴스와 전체 구성은 거의 동일합니다.

다만 Orders DB 인스턴스는 `"Engine": "aurora-postgresql"`을 사용하고, Catalog DB 인스턴스는 `"Engine": "aurora-mysql"`을 사용합니다. 또한 `"DBClusterIdentifier"`에서 참조하는 DB 클러스터가 서로 다릅니다.

그 외 `"DBInstanceClass": "db.t3.medium"`, `"PromotionTier": 0`, `"PubliclyAccessible": false`, `"DependsOn"`, `"UpdateReplacePolicy": "Delete"`, `"DeletionPolicy": "Delete"` 구성은 동일합니다.

---
## 7. `"NetworkRedisCluster8A158537"`

```json
  "NetworkRedisCluster8A158537": {
   "Type": "AWS::ElastiCache::ServerlessCache",
   "Properties": {
    "Description": "Redis cluster for checkout service",
    "Engine": "redis",
    "SecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkRedisSecurityGroupF069D970",
       "GroupId"
      ]
     }
    ],
    "ServerlessCacheName": "retail-store-ecs-redis",
    "SubnetIds": [
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638"
     },
     {
      "Ref": "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173"
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
    ]
   },
   "DeletionPolicy": "Delete"
  },
```

체크 아웃 서비스를 위한 Redis 캐시를 생성합니다.

---

Redis 캐시 리소스에도 `"UpdateReplacePolicy"` 속성 자체는 사용할 수 있습니다. `"UpdateReplacePolicy"`는 특정 RDS 리소스 전용 옵션이 아니라, CloudFormation 리소스에 공통적으로 붙일 수 있는 리소스 속성입니다.

다만 이 코드에서는 `"UpdateReplacePolicy"`를 명시하지 않았고, `"DeletionPolicy": "Delete"`만 명시했습니다. 따라서 스택 삭제 시 Redis 캐시는 함께 삭제되도록 설정되어 있지만, 스택 업데이트 중 리소스 교체가 발생했을 때 기존 Redis 캐시를 어떻게 처리할지는 별도로 명시하지 않은 상태입니다.

---
## 8. `"NetworkCartsTableVPCLattice03AD316C"`

```json
  "NetworkCartsTableVPCLattice03AD316C": {
   "Type": "AWS::DynamoDB::Table",
   "Properties": {
    "AttributeDefinitions": [
     {
      "AttributeName": "id",
      "AttributeType": "S"
     },
     {
      "AttributeName": "customerId",
      "AttributeType": "S"
     }
    ],
    "BillingMode": "PAY_PER_REQUEST",
    "GlobalSecondaryIndexes": [
     {
      "IndexName": "idx_global_customerId",
      "KeySchema": [
       {
        "AttributeName": "customerId",
        "KeyType": "HASH"
       }
      ],
      "Projection": {
       "ProjectionType": "ALL"
      }
     }
    ],
    "KeySchema": [
     {
      "AttributeName": "id",
      "KeyType": "HASH"
     }
    ],
    "TableName": "retail-store-ecs-carts-vpclattice",
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
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  }
```

`"NetworkCartsTableVPCLattice03AD316C"`는 Cart 서비스가 사용할 DynamoDB 테이블을 생성하는 리소스입니다. Aurora/RDS와 달리 DynamoDB에는 사용자가 직접 관리하는 클러스터 개념이 없습니다. DynamoDB는 AWS의 완전 관리형 서버리스 NoSQL 데이터베이스이기 때문에, 사용자는 클러스터나 인스턴스를 직접 구성하지 않습니다. 대신 테이블, 기본 키, 인덱스, 과금 방식 같은 논리적인 설정을 정의합니다.

---

```json
    "AttributeDefinitions": [
     {
      "AttributeName": "id",
      "AttributeType": "S"
     },
     {
      "AttributeName": "customerId",
      "AttributeType": "S"
     }
    ],
```

`"AttributeDefinitions"`는 DynamoDB 테이블이나 인덱스에서 키로 사용할 속성의 이름과 타입을 정의하는 설정입니다. 여기서 `"AttributeType": "S"`는 이 속성의 타입이 String, 즉 문자열임을 의미합니다.

주의할 점은 `"AttributeDefinitions"`가 테이블의 모든 속성을 정의하는 곳은 아니라는 것입니다. DynamoDB에서는 기본 키나 인덱스 키로 사용되는 속성만 `"AttributeDefinitions"`에 정의합니다.

---

`"BillingMode": "PAY_PER_REQUEST"`는 DynamoDB의 과금 방식 중 하나인 온디맨드(on-demand)를 사용하겠다는 설정입니다. 온디맨드 과금 방식은 미리 읽기/쓰기 처리량을 지정하지 않고, 실제 발생한 요청 수에 따라 비용을 지불하는 방식입니다.

또 다른 방식으로는 `"PROVISIONED"` 과금 방식이 있습니다. 이 방식은 미리 읽기/쓰기 처리량을 정해두는 방식으로, 어느 정도 트래픽 양을 예측할 수 있을 때 선호됩니다.

---

```json
    "GlobalSecondaryIndexes": [
     {
      "IndexName": "idx_global_customerId",
      "KeySchema": [
       {
        "AttributeName": "customerId",
        "KeyType": "HASH"
       }
      ],
      "Projection": {
       "ProjectionType": "ALL"
      }
     }
    ],
```

`"GlobalSecondaryIndexes"`는 DynamoDB 테이블에 Global Secondary Index(GSI)를 추가하는 설정입니다. GSI는 기본 키가 아닌 다른 속성을 기준으로 데이터를 조회할 수 있게 해주는 보조 인덱스입니다. 이 테이블의 기본 키는 `"id"`이지만, `"idx_global_customerId"` GSI를 추가하여 `"customerId"` 기준으로도 데이터를 조회할 수 있게 구성합니다. 

---

`"KeyType"`에는 `"HASH"` 또는 `"RANGE"`가 들어갈 수 있습니다. `"HASH"`는 파티션 키를 의미하고, `"RANGE"`는 정렬 키를 의미합니다. 

파티션 키는 DynamoDB에서 데이터를 찾기 위한 대표 주소입니다. 이 테이블에서는 `"id"`가 파티션 키이므로, DynamoDB는 `"id"` 값을 기준으로 장바구니 데이터를 빠르게 찾을 수 있습니다.

예를 들면:

```
{id: "cart-001", customerId: "user-1", items: [...]}
{id: "cart-002", customerId: "user-2", items: [...]}
{id: "cart-003", customerId: "user-1", items: [...]}
```

여기서 기본 테이블의 파티션 키가 `"id"`이기 때문에, 이 DynamoDB 테이블은 `"id"`를 기준으로 데이터를 찾는 데 강합니다.

또한 이 코드에서는 `"customerId"`를 파티션 키로 사용하는 GSI도 함께 생성합니다. 따라서 기본 테이블은 `"id"` 기준 조회에 강하고, GSI를 사용하면 `"customerId"` 기준 조회도 효율적으로 수행할 수 있습니다.

정렬 키는 같은 파티션 키를 가진 데이터 안에서 정렬이나 범위 조회에 사용할 수 있는 키입니다.

예를 들어 주문 테이블이 아래처럼 구성되어 있다고 가정해보겠습니다.

```json
"KeySchema": [  
 {  
  "AttributeName": "customerId",  
  "KeyType": "HASH"  
 },  
 {  
  "AttributeName": "createdAt",  
  "KeyType": "RANGE"  
 }  
]
```

이 경우 `"customerId"`는 파티션 키이고, `"createdAt"`은 정렬 키입니다.

예를 들면 데이터가 아래처럼 저장될 수 있습니다.

```
{customerId: "user-1", createdAt: "2026-04-01T10:00:00", orderId: "order-001"}
{customerId: "user-1", createdAt: "2026-04-03T15:30:00", orderId: "order-002"}
{customerId: "user-2", createdAt: "2026-04-02T09:00:00", orderId: "order-003"}
```

여기서 파티션 키가 `"customerId"`이기 때문에, DynamoDB는 고객별로 데이터를 찾는 데 강합니다. 그리고 정렬 키가 `"createdAt"`이기 때문에, 같은 고객의 주문 안에서 생성 시간 기준으로 정렬하거나 범위 조회를 할 수 있습니다.

예를 들어 `"customerId"`가 `"user-1"`인 주문 중에서 `"createdAt"`이 `"2026-04-01"` 이후인 주문만 조회하는 식의 접근이 가능합니다.

---

`"ProjectionType": "ALL"`은 이 인덱스에 원본 테이블의 모든 속성을 포함하겠다는 의미입니다. 따라서 `"customerId"`로 조회할 때 인덱스만으로도 필요한 데이터를 가져올 수 있습니다.

---

```json
    "KeySchema": [
     {
      "AttributeName": "id",
      "KeyType": "HASH"
     }
    ]
```

`"KeySchema"`는 이 테이블에서 어떤 속성을 기본 키로 사용할지 정합니다. 여기서는 `"AttributeName": "id"`이고 `"KeyType": "HASH"`이므로, `"id"`를 파티션 키로 사용한다는 뜻입니다.
