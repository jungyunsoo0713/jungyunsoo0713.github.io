---
title: "02. 데이터베이스 및 스토리지 (RDS, Redis, DynamoDB)"
weight: 2
---
> **작성일:** 2026-05-04 | **수정일:** 2026-05-04
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

DB 서브넷 그룹을 생성하는 리소스입니다. DB 서브넷 그룹은 DB 클러스터가 사용할 VPC의 서브넷들을 지정합니다. 이 DB 서브넷 그룹은 Orders 서비스의 DB 클러스터를 위한 리소스입니다. DB 클러스터란 DB를 구동시키기 위한 DB 인스턴스와 관련 설정들을 포함한 논리적 묶음입니다. 이 DB 서브넷 그룹은 Orders 서비스용 DB 클러스터 입니다.

---

`"SubnetIds"`로 DB 클러스터가 사용할 서브넷들을 지정합니다. 여기서는 총 3개의 프라이빗 서브넷이 지정되었습니다.

참고로 엄밀히 말하면 프라이빗 서브넷에 위치하는 것은 DB 클러스터가 아니라 DB 인스턴스입니다.

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

DB 클러스터를 생성하는 리소스입니다. Orders 서비스에서 사용할 Aurora PostgreSQL DB 클러스터입니다.

---

`"CopyTagsToSnapshot"`는 DB 클러스터에 붙어있는 태그들을 DB 스냅샷에도 복사할지를 정합니다. 값이 `true`이기 때문에 이 DB 클러스터가 가지고 있는 태그들은 스냅샷에도 복사되어 저장됩니다. 스냅샷이란 DB 클러스터 안의 테이블 구조와 테이블 안의 데이터를 특정 시점 기준으로 복사해 저장하는 것을 의미합니다.

---

`"DBClusterIdentifier"`는 이 DB 클러스터를 식별하는 식별자입니다. 식별자의 값은 `"retail-store-ecs-orders"`입니다.

---

`"DBClusterParameterGroupName"`는 DB 클러스터에 적용할 파라미터 그룹 이름을 지정합니다. 파라미터 그룹은 DB의 설정값 묶음입니다. 다른 파라미터 그룹을 사용하면 다른 설정값을 줄 수 있습니다. 이 필드의 값은 `"default.aurora-postgresql17"`입니다.

---

`"DBSubnetGroupName"`는 DB 서브넷 그룹을 지정합니다. 여기서는 앞에서 생성했던 DB 서브넷 그룹을 지정합니다. 

---

`"DatabaseName"`은 이 데이터베이스의 이름을 지정합니다. 참고로 데이터베이스는 DB 클러스터 안에 존재하는 논리적 데이터 저장 공간이고, DB 인스턴스는 데이터베이스를 작동시키는 PostgreSQL 같은 DB 엔진을 실행하는 서버입니다. 실무에서는 DB라는 표현을 DB 클러스터, DB 인스턴스, 데이터베이스를 뭉뚱그려 지칭할 때도 사용합니다. 일반적으로 DB 클러스터 내부에서 데이터를 저장하는 논리적 공간은 "DB"가 아니라 "데이터베이스"라고 부르는 것이 더 정확합니다.

---

`"DeletionProtection": false`는 이 DB 클러스터의 삭제 보호가 비활성화되어 있다는 것을 의미합니다. DB에는 중요한 데이터들이 있을 수 있기 때문에 `"DeletionProtection"` 옵션으로 삭제 보호를 해줄 수 있습니다. 실무 환경에서는 보통 이 옵션이 `true`로 설정되어 있는 경우가 많습니다.

---

`"Engine"`은 데이터베이스를 작동시키는 DB 엔진을 지정합니다. 여기서는 `"aurora-postgresql"`을 사용합니다.

---

`"EngineVersion"`은 DB 엔진의 버전을 지정합니다. 여기서는 `17.4` 버전을 사용합니다.

---

`"MasterUserPassword"`는 DB에 접근할 때 필요한 마스터 사용자의 비밀번호를 지정합니다.

`resolve:secretsmanager`에서 `resolve`는 외부에 저장된 값을 가져온다는 것을 의미합니다. `secretsmanager`는 AWS Secrets Manager 서비스를 의미합니다. 이 서비스는 애플리케이션에 필요한 기밀 데이터를 저장하고 관리합니다.

`"NetworkDBCredentialsSecretADE5ABB3"`는 Secrets Manager의 Secret 리소스의 Logical ID입니다. 이 리소스에는 DB의 기밀 데이터인 마스터 사용자의 이름과 비밀번호가 들어 있습니다. `"Ref"`로 이 리소스를 참조합니다.

`:SecretString:password::`는 위 Secret 리소스의 `SecretString`에서 `password` 값을 꺼낸다는 의미입니다.

`"Fn::Join"`으로 이를 다 묶으면, 최종 문자열은 `{{resolve:secretsmanager:retail-store-ecs-db-credentials:SecretString:password::}}`와 같은 형태가 됩니다.

이후 이 템플릿이 스택으로 생성될 때, 이 문자열이 마스터 사용자의 비밀번호 값으로 해석됩니다.

---

`"MasterUsername"`도 위와 같은 방식으로 마스터 사용자의 이름을 가져옵니다.

---

`"Port"`는 이 DB 클러스터가 사용하는 포트 번호를 의미합니다. 값인 `5432`는 PostgreSQL의 기본 포트 번호입니다. 이 필드를 명시하지 않으면 해당 DB 클러스터가 사용하는 DB 엔진의 기본 포트 번호가 사용됩니다.

---

`"StorageEncrypted": true`는 이 DB 클러스터의 스토리지를 암호화한다는 것을 의미합니다. 스토리지는 데이터를 저장하는 저장 계층을 의미합니다. 이 옵션이 활성화되면, 데이터가 스토리지에 기록될 때 암호화되어 기록됩니다. 정상적인 경로로 스토리지에 접근한다면 복호화된 데이터를 볼 수 있습니다. 하지만 비정상적인 경로로 스토리지에 접근하면 암호화된 데이터만 볼 수 있습니다. 

---

`"VpcSecurityGroupIds"`는 이 DB 클러스터가 사용할 보안 그룹의 ID를 지정합니다. Orders DB 클러스터에 적용되는 RDS용 보안 그룹인 `"NetworkOrdersRDSSecurityGroupD378DF87"`의 보안 그룹 ID를 가져와 지정합니다.

---

`"UpdateReplacePolicy"`는 이 DB 클러스터를 관리하는 CloudFormation 스택이 업데이트 중에 이 DB 클러스터를 새 리소스로 교체할 때, 기존 DB 클러스터의 보존 여부를 정합니다. 필드의 값이 `"Delete"`이기 때문에, 기존 DB 클러스터를 보존하지 않습니다.

`"UpdateReplacePolicy"`의 값에는 `"Retain"`, `"Snapshot"` 또한 존재합니다. `"Retain"`은 기존 DB 클러스터를 보존하는 옵션이고, `"Snapshot"`은 기존 DB 클러스터는 삭제하지만, DB 클러스터의 스냅샷은 생성해 보존합니다. 옵션을 따로 지정해주지 않는다면 기본값은 `"Snapshot"`입니다.

---

`"DeletionPolicy"`는 이 DB 클러스터를 관리하는 CloudFormation 스택이 삭제될 때 DB 클러스터의 보존 여부를 정합니다. 필드의 값이 `"Delete"`이기 때문에, 기존 DB 클러스터를 보존하지 않습니다. 

`"DeletionPolicy"`의 값에는 `"Delete"`, `"Retain"`, `"Snapshot"`, `"RetainExceptOnCreate"`가 올 수 있습니다. `"Retain"`은 기존 DB 클러스터를 보존하는 옵션이고, `"Snapshot"`은 기존 DB 클러스터는 삭제하지만, DB 클러스터의 스냅샷은 생성해 보존합니다. `"RetainExceptOnCreate"`는 스택 생성에 실패하여 롤백하는 경우를 제외하고 기존 DB 클러스터를 보존합니다. `"DeletionPolicy"` 옵션을 따로 지정해주지 않는다면 기본값은 `"Snapshot"`입니다.

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

DB 인스턴스를 생성하는 리소스입니다. 이 DB 인스턴스는 Orders 서비스용 DB 클러스터에 연결되는 DB 인스턴스입니다. DB 인스턴스란 DB 엔진이 실행되어 쿼리를 처리하는 서버를 의미합니다. 이 DB 인스턴스는 Writer DB 인스턴스로, 쓰기 쿼리(`INSERT`,` UPDATE`,` DELETE `등)와 읽기 쿼리를 처리합니다. Reader DB 인스턴스도 존재하며, Reader DB 인스턴스는 주로 읽기 쿼리를 처리합니다.

일반적으로 Aurora DB 클러스터는 1개의 Writer DB 인스턴스와 0개 이상의 Reader DB 인스턴스로 구성되어 있습니다. Writer DB 인스턴스는 쓰기 쿼리뿐만 아니라 읽기 쿼리도 처리할 수 있기 때문에, 이론상 하나의 Writer DB 인스턴스만 존재해도 기능에는 지장이 없습니다.

Writer DB 인스턴스는 일반적으로 하나만 존재합니다. 여러 Writer DB 인스턴스가 동시에 쓰기 쿼리를 처리하면 데이터 정합성 관리가 복잡해질 수 있기 때문에, 일반적으로 하나의 Writer DB 인스턴스만 사용됩니다. 반면 읽기 쿼리는 데이터를 변경하지 않고, 일반적으로 읽기 요청이 쓰기 요청보다 많기 때문에 여러 개의 Reader DB 인스턴스를 두는 것이 일반적입니다.

참고로 하나의 DB 인스턴스도 여러 쓰기 쿼리를 동시에 처리할 수 있습니다. 하지만 여러 Writer DB 인스턴스가 동시에 쓰기 쿼리를 처리하면 데이터 정합성 관리가 복잡해지기 때문에, Aurora DB 클러스터는 일반적으로 하나의 Writer DB 인스턴스만 사용합니다.

만약 Writer DB 인스턴스에 장애가 발생하면 기존 Reader DB 인스턴스 중 하나를 Writer DB 인스턴스로 승격시켜 그 기능을 대체할 수 있습니다.

---

`"DBClusterIdentifier"`는 이 DB 인스턴스가 연결될 DB 클러스터를 지정합니다. 앞서 생성한 Orders 서비스용 DB 클러스터를 지정합니다.

---

`"DBInstanceClass"`는 이 DB 인스턴스의 클래스를 지정합니다. 클래스는 이 DB 인스턴스가 사용하는 서버의 CPU, 메모리 등의 하드웨어 스펙을 의미합니다. 

이 필드의 값인 `"db.t3.medium"`은 RDS DB 인스턴스의 클래스입니다. 여기서 `db`는 RDS DB 인스턴스용 클래스라는 것을 의미하고, `t3`는 인스턴스 패밀리를 의미합니다. `t3`는 비교적 작은 규모에서 사용하는 범용 버스트 성능 인스턴스 계열입니다. 버스트 성능은 평소에는 기본 성능으로 동작하다가, 필요할 때 CPU 크레딧을 사용해 일시적으로 더 높은 성능을 내는 방식입니다. `medium`은 `t3` 계열 안에서의 인스턴스 크기를 의미하며, 중간 정도의 크기를 나타냅니다. 크기가 커질수록 CPU 성능과 메모리 용량이 증가합니다.

---

`"Engine"`은 이 DB 인스턴스가 사용하는 DB 엔진을 지정합니다. 값은 `"aurora-postgresql"`입니다. 이 값은 DB 클러스터에 지정된 `"Engine"` 필드의 값과 동일해야 합니다. Aurora처럼 DB 클러스터와 DB 인스턴스를 함께 사용하는 구조에서는, DB 클러스터의 `"Engine"` 값과 해당 DB 클러스터에 연결된 DB 인스턴스의 `"Engine"` 값은 동일해야 합니다.

---

`"PromotionTier"`는 Writer DB 인스턴스 장애 시 Reader DB 인스턴스의 승격 우선순위를 정합니다. 필드의 값이 낮을수록 우선순위가 높습니다. 이 리소스는 이미 Writer DB 인스턴스이므로, 이 필드는 사실상 의미가 없습니다.

---

`"PubliclyAccessible"`는 이 DB 인스턴스를 인터넷에서 접근 가능하게 만들지 정하는 필드입니다. 필드의 값이 `false`이기 때문에 이 DB 인스턴스는 인터넷에서 접근이 가능하지 않습니다. 일반적으로 DB는 인터넷에서 직접 접근하지 않도록 구성합니다. 일부 DB는 인터넷에서 접근 가능하게 설정할 수 있지만, 보안상 권장되지 않습니다.

---

`"DependsOn"`을 보면 이 DB 인스턴스가 생성되기 전에 먼저 생성되어야 할 리소스들이 지정되어 있습니다. 여기서는 프라이빗 서브넷의 기본 라우트와 라우트 테이블 어소시에이션이 먼저 생성되어야 합니다. 이는 DB 인스턴스가 프라이빗 서브넷에 위치하기 때문에, 프라이빗 서브넷의 네트워크 구성이 먼저 선행되어야 이 DB 인스턴스를 배치할 수 있음을 의미합니다.

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

DB 서브넷 그룹을 생성하는 리소스입니다. 이 DB 서브넷 그룹은 Catalog 서비스용 DB 클러스터에서 사용할 DB 서브넷 그룹입니다. 앞서 생성한 Orders 서비스용 DB 서브넷 그룹과 동일한 구조를 가지고 있습니다.

Orders 서비스용 DB 서브넷 그룹이 지정하는 서브넷들과 동일한 서브넷들을 지정하고 있습니다.

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

DB 클러스터를 생성하는 리소스입니다. Catalog 서비스에서 사용할 Aurora MySQL DB 클러스터입니다. 앞서 생성한 Orders 서비스용 DB 클러스터와 동일한 구조를 가지고 있습니다. 다만 Catalog 서비스용 DB 클러스터가 MySQL을 사용하는 반면, Orders 서비스용 DB 클러스터는 PostgreSQL을 사용합니다.

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

DB 인스턴스를 생성하는 리소스입니다. 이 DB 인스턴스는 Catalog 서비스용 DB 클러스터에 연결되는 DB 인스턴스입니다.

앞서 생성한 Orders 서비스용 DB 인스턴스와 동일한 구조를 가지고 있습니다. 다만 Catalog 서비스용 DB 인스턴스가 MySQL을 사용하는 반면, Orders 서비스용 DB 인스턴스는 PostgreSQL을 사용합니다.

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

Redis Serverless 캐시를 생성하는 리소스입니다. Checkout 서비스에서 사용할 Redis Serverless 캐시입니다. Redis Serverless 캐시는 Redis 캐시 환경을 생성하지만, 노드, 샤드, 클러스터 구성 등을 사용자가 직접 관리하지 않는 리소스입니다. 이 리소스는 Redis 캐시를 생성함과 동시에 DB 서브넷 그룹처럼 이 캐시가 쓰일 서브넷들도 지정합니다. 

Orders 서비스용 DB 서브넷 그룹이 지정하는 서브넷들과 동일한 서브넷들을 지정하고 있습니다.

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

DynamoDB 테이블을 생성하는 리소스입니다. DynamoDB는 AWS의 관리형 NoSQL DB 서비스입니다. Carts 서비스에서 사용할 DynamoDB 테이블 입니다. 

DynamoDB 테이블이 Carts 서비스에서 사용되는 이유는 데이터 조회 패턴이 어느 정도 뚜렷하기 때문입니다. DynamoDB는 정해진 키 기준으로 빠르게 조회하는 데 강합니다. 예를 들어, `customerId`로 특정 고객의 장바구니를 찾는 것처럼 일관적인 조회 패턴이 존재합니다. 반면 RDS는 복잡한 쿼리 등이 많이 사용되거나 트랜잭션 기능이 필요할 때 더 적합합니다.

---

`"AttributeDefinitions"`는 DynamoDB 테이블에서 키로 사용할 속성과 속성 타입들을 정의합니다. 이 모든 속성들은 테이블의 기본 키나 인덱스 키로 사용됩니다.

`"AttributeName": "id"`는 속성의 이름이 `"id"`임을 의미합니다.
`"AttributeType": "S"`는 해당 속성의 타입이 문자열이라는 것을 의미합니다.

`"AttributeName": "customerId"`는 속성의 이름이 `"customerId"`임을 의미합니다.
`"AttributeType": "S"`는 해당 속성의 타입이 문자열이라는 것을 의미합니다.

---

`"BillingMode"`는 이 DynamoDB 테이블의 사용 비용이 어떤 방식으로 계산될지를 정합니다. 이 필드의 값인 `"PAY_PER_REQUEST"`는 읽기/쓰기 요청량에 따라 비용이 부과됨을 의미합니다. 즉 `"PAY_PER_REQUEST"` 방식은 미리 처리 용량을 정하지 않고, 사용량에 비례해서 사용 비용을 지불하는 방식입니다. 

`"PROVISIONED"` 과금 방식도 존재합니다. 이 방식은 미리 처리 용량을 정해놓기 때문에 트래픽이 어느 정도 예측 가능할 때 주로 사용됩니다.

---

`"GlobalSecondaryIndexes"`는 DynamoDB 테이블에 추가하는 보조 인덱스를 설정합니다. 여기서 인덱스란, 데이터 조회를 빠르게 하기 위해 만드는 검색용 구조입니다. 따라서 이 필드는 기본 키 외에 추가적인 검색용 구조를 설정한다고 볼 수 있습니다.

`"IndexName"`는 추가하는 보조 인덱스의 이름입니다. `"idx_global_customerId"`가 이름으로 지정됩니다. 

`"KeySchema"`는 보조 인덱스에서 데이터를 조회할 때 사용할 키를 정합니다. 여기서는 `"GlobalSecondaryIndexes"` 안의 `"KeySchema"`를 말한 것입니다. 다른 곳에서 사용되는 `"KeySchema"`도 키를 정하는 역할은 같지만, 대상이 테이블인지 보조 인덱스인지에 따라 어떤 키를 정하는지가 달라질 수 있습니다.

`"AttributeName": "customerId"`를 보면 이 보조 인덱스에서 사용할 키, 즉 속성의 이름이 `"customerId"`임을 알 수 있습니다.

`"KeyType": "HASH"`는 이 보조 인덱스의 키가 파티션 키라는 것을 의미합니다. 파티션 키는 데이터를 조회할 때 기준이 되는 키를 의미합니다. 이 파티션 키를 기준으로 데이터를 빠르게 조회할 수 있습니다. 다른 속성을 기준으로도 데이터를 조회할 수 있지만, 파티션 키를 기준으로 조회하는 것보다 느릴 수 있습니다.

`"Projection"`은 보조 인덱스에 어떤 속성들을 복사해 넣을지 정하는 필드입니다. 보조 인덱스는 원본 테이블의 모든 속성을 가질 수도 있고, 일부 속성만 가질 수도 있습니다. 이 필드의 값이 `"ALL"`이기 때문에 보조 인덱스도 원본 테이블에 있는 모든 속성을 가집니다.

---

`"KeySchema"`는 어떤 속성을 키로 사용할지 정합니다. `"AttributeName": "id"`로 `"id"` 속성이 키로 사용됨을 의미합니다. `"KeyType": "HASH"`이기 때문에 이 키는 파티션 키로 사용됩니다.

---

`"TableName": "retail-store-ecs-carts-vpclattice"`는 이 DynamoDB 테이블의 이름이 `"retail-store-ecs-carts-vpclattice"`임을 의미합니다.



