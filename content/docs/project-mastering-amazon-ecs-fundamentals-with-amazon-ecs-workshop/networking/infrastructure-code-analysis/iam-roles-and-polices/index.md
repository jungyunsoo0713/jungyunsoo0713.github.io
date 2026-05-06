---
title: "05. 권한 관리 (IAM Roles & Policies)"
weight: 5
---
> **작성일:** 2026-05-06 | **수정일:** 2026-05-06
```json
{
  "NetworkOrdersEcsTaskExecutionRole0CFAB02D": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Orders Task Execution Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
       ]
      ]
     }
    ],
    "RoleName": "ordersEcsTaskExecutionRole",
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
   }
  },
  "NetworkOrdersEcsTaskExecutionRoleDefaultPolicy1D6EAD89": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "kms:Decrypt",
        "secretsmanager:GetSecretValue",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": [
        {
         "Fn::GetAtt": [
          "NetworkKMSKey37A7DAB2",
          "Arn"
         ]
        },
        {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":ssm:",
           {
            "Ref": "AWS::Region"
           },
           ":",
           {
            "Ref": "AWS::AccountId"
           },
           ":parameter",
           {
            "Ref": "NetworkDBEndpointParameter9BA21FDB"
           }
          ]
         ]
        },
        {
         "Ref": "NetworkDBCredentialsSecretADE5ABB3"
        }
       ]
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkOrdersEcsTaskExecutionRoleDefaultPolicy1D6EAD89",
    "Roles": [
     {
      "Ref": "NetworkOrdersEcsTaskExecutionRole0CFAB02D"
     }
    ]
   }
  },
  "NetworkCheckoutEcsTaskExecutionRole8AE623E4": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Checkout Task Execution Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
       ]
      ]
     }
    ],
    "RoleName": "checkoutEcsTaskExecutionRole",
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
   }
  },
  "NetworkCheckoutEcsTaskExecutionRoleDefaultPolicyA6E53600": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "kms:Decrypt",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": [
        {
         "Fn::GetAtt": [
          "NetworkKMSKey37A7DAB2",
          "Arn"
         ]
        },
        {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":ssm:",
           {
            "Ref": "AWS::Region"
           },
           ":",
           {
            "Ref": "AWS::AccountId"
           },
           ":parameter",
           {
            "Ref": "NetworkRedisEndpointParameter558525EA"
           }
          ]
         ]
        }
       ]
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkCheckoutEcsTaskExecutionRoleDefaultPolicyA6E53600",
    "Roles": [
     {
      "Ref": "NetworkCheckoutEcsTaskExecutionRole8AE623E4"
     }
    ]
   }
  },
  "NetworkCatalogEcsTaskExecutionRoleF3ED238D": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Catalog Task Execution Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
       ]
      ]
     }
    ],
    "RoleName": "catalogEcsTaskExecutionRole",
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
   }
  },
  "NetworkCatalogEcsTaskExecutionRoleDefaultPolicyF6FAB6DA": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "kms:Decrypt",
        "secretsmanager:GetSecretValue",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": [
        {
         "Fn::GetAtt": [
          "NetworkKMSKey37A7DAB2",
          "Arn"
         ]
        },
        {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":ssm:",
           {
            "Ref": "AWS::Region"
           },
           ":",
           {
            "Ref": "AWS::AccountId"
           },
           ":parameter",
           {
            "Ref": "NetworkCatalogDBEndpointParameterF3C95C74"
           }
          ]
         ]
        },
        {
         "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
        }
       ]
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkCatalogEcsTaskExecutionRoleDefaultPolicyF6FAB6DA",
    "Roles": [
     {
      "Ref": "NetworkCatalogEcsTaskExecutionRoleF3ED238D"
     }
    ]
   }
  },
  "NetworkECSInfraRoleVpcLattice46E30754": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS VPC Lattice Infra Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/AmazonECSInfrastructureRolePolicyForVpcLattice"
       ]
      ]
     }
    ],
    "RoleName": "ECSInfraRoleVpcLattice",
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
   }
  },
  "NetworkCartsEcsVpcLatticeTaskRole2E197958": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Carts Task Role for VPC Lattice Lab",
    "RoleName": "ECSImmesionDay_CartsVpcLatticeTaskRole",
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
   }
  },
  "NetworkCartsEcsVpcLatticeTaskRoleDefaultPolicy28F12718": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": "dynamodb:*",
       "Effect": "Allow",
       "Resource": "*"
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkCartsEcsVpcLatticeTaskRoleDefaultPolicy28F12718",
    "Roles": [
     {
      "Ref": "NetworkCartsEcsVpcLatticeTaskRole2E197958"
     }
    ]
   }
  },
  "NetworkECSServiceConnectCertificateRole144192CD": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "IAM Role for ECS Service Connect to issue certificates from AWS Private CA",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSInfrastructureRolePolicyForServiceConnectTransportLayerSecurity"
       ]
      ]
     }
    ],
    "RoleName": "ecs-service-connect-certificate-role",
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
   }
  }
}
```

---

## 1. `"NetworkOrdersEcsTaskExecutionRole0CFAB02D"`

```json
  "NetworkOrdersEcsTaskExecutionRole0CFAB02D": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Orders Task Execution Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
       ]
      ]
     }
    ],
    "RoleName": "ordersEcsTaskExecutionRole",
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
   }
  },
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 Orders 서비스의 Task를 실행할 때 사용되는 Task Execution Role입니다.

---

## 2. `"NetworkOrdersEcsTaskExecutionRoleDefaultPolicy1D6EAD89"`

```json
  "NetworkOrdersEcsTaskExecutionRoleDefaultPolicy1D6EAD89": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "kms:Decrypt",
        "secretsmanager:GetSecretValue",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": [
        {
         "Fn::GetAtt": [
          "NetworkKMSKey37A7DAB2",
          "Arn"
         ]
        },
        {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":ssm:",
           {
            "Ref": "AWS::Region"
           },
           ":",
           {
            "Ref": "AWS::AccountId"
           },
           ":parameter",
           {
            "Ref": "NetworkDBEndpointParameter9BA21FDB"
           }
          ]
         ]
        },
        {
         "Ref": "NetworkDBCredentialsSecretADE5ABB3"
        }
       ]
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkOrdersEcsTaskExecutionRoleDefaultPolicy1D6EAD89",
    "Roles": [
     {
      "Ref": "NetworkOrdersEcsTaskExecutionRole0CFAB02D"
     }
    ]
   }
  },
```

IAM Policy를 생성하는 리소스입니다. 이 IAM Policy는 앞서 생성한 Orders 서비스의 Task를 실행할 때 사용되는 Task Execution Role에 연결됩니다.

---

`"Action"`을 보면 Secrets Manager의 시크릿 값을 읽을 수 있고, KMS 키를 사용해 데이터 키를 복호화할 수 있으며, Systems Manager Parameter Store에 저장된 파라미터 값을 읽을 수 있습니다.

이 작업들이 필요한 이유는, Orders 서비스가 DB 클러스터에 접속하기 위해 Secrets Manager에 저장된 DB Credentials(DB 시크릿)가 필요하고, 이 과정에서 KMS 키로 암호화되어 저장된 데이터 키를 복호화해야 하기 때문입니다. 또한 DB에 접속하기 위해 Systems Manager Parameter Store에 저장된 DB 엔드포인트 값도 필요합니다.

Secrets Manager는 시크릿을 저장할 때 암호화해서 저장합니다. Secrets Manager가 KMS에 데이터 키를 요청합니다. KMS는 데이터 키와 KMS 키로 보호된 데이터 키를 Secrets Manager에게 전달합니다. Secrets Manager는 이 데이터 키로 시크릿을 암호화하여 저장한 후 데이터 키를 파기합니다. 이때 Secrets Manager는 암호화된 시크릿과 KMS 키로 보호된 데이터 키를 가지고 있습니다. 이 시크릿을 복호화해야 할 때, Secrets Manager가 KMS에 KMS 키로 보호된 데이터 키를 전달하면, KMS는 이 데이터 키를 복호화해서 Secrets Manager에게 다시 전달합니다. Secrets Manager는 전달받은 데이터 키로 시크릿을 복호화하고, 전달받은 데이터 키를 파기합니다.

---

`"Resource"`는 이전에 생성되었던 KMS 키, DB 엔드포인트 파라미터, DB 시크릿을 지정합니다. `"Action"`에 필요한 리소스만 지정되었음을 알 수 있습니다.

---

## 3. `"NetworkCheckoutEcsTaskExecutionRole8AE623E4"`

```json
  "NetworkCheckoutEcsTaskExecutionRole8AE623E4": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Checkout Task Execution Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
       ]
      ]
     }
    ],
    "RoleName": "checkoutEcsTaskExecutionRole",
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
   }
  },
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 Checkout 서비스의 Task를 실행할 때 사용되는 Task Execution Role입니다.

---

## 4. `"NetworkCheckoutEcsTaskExecutionRoleDefaultPolicyA6E53600"`

```json
  "NetworkCheckoutEcsTaskExecutionRoleDefaultPolicyA6E53600": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "kms:Decrypt",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": [
        {
         "Fn::GetAtt": [
          "NetworkKMSKey37A7DAB2",
          "Arn"
         ]
        },
        {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":ssm:",
           {
            "Ref": "AWS::Region"
           },
           ":",
           {
            "Ref": "AWS::AccountId"
           },
           ":parameter",
           {
            "Ref": "NetworkRedisEndpointParameter558525EA"
           }
          ]
         ]
        }
       ]
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkCheckoutEcsTaskExecutionRoleDefaultPolicyA6E53600",
    "Roles": [
     {
      "Ref": "NetworkCheckoutEcsTaskExecutionRole8AE623E4"
     }
    ]
   }
  },
```

IAM Policy를 생성하는 리소스입니다. 이 IAM Policy는 앞서 생성한 Checkout 서비스의 Task Execution Role에 연결됩니다. Orders 서비스의 Task Execution Role에 연결되는 IAM Policy와 다르게, `"Action"`에 `"secretsmanager:GetSecretValue"` 값이 존재하지 않습니다. 이는 Redis Cache에 접근할 때 별도의 시크릿을 사용하지 않기 때문입니다.

따라서 `"Resource"`에도 DB 엔드포인트 파라미터를 제외한, KMS 키, DB 시크릿이 지정 되었습니다. 

참고로 `"kms:Decrypt"`는 여전히 포함되어 있는데, 이는 AWS CDK가 공통적으로 추가한 패턴인 것으로 추측됩니다. 이 권한은 실제로 사용되지 않을 가능성이 높습니다.

---

## 5. `"NetworkCatalogEcsTaskExecutionRoleF3ED238D"`

```json
  "NetworkCatalogEcsTaskExecutionRoleF3ED238D": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Catalog Task Execution Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
       ]
      ]
     }
    ],
    "RoleName": "catalogEcsTaskExecutionRole",
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
   }
  },
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 Catalog 서비스의 Task를 실행할 때 사용되는 Task Execution Role입니다.

---

## 6. `"NetworkCatalogEcsTaskExecutionRoleDefaultPolicyF6FAB6DA"`

```json
  "NetworkCatalogEcsTaskExecutionRoleDefaultPolicyF6FAB6DA": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "kms:Decrypt",
        "secretsmanager:GetSecretValue",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": [
        {
         "Fn::GetAtt": [
          "NetworkKMSKey37A7DAB2",
          "Arn"
         ]
        },
        {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":ssm:",
           {
            "Ref": "AWS::Region"
           },
           ":",
           {
            "Ref": "AWS::AccountId"
           },
           ":parameter",
           {
            "Ref": "NetworkCatalogDBEndpointParameterF3C95C74"
           }
          ]
         ]
        },
        {
         "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
        }
       ]
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkCatalogEcsTaskExecutionRoleDefaultPolicyF6FAB6DA",
    "Roles": [
     {
      "Ref": "NetworkCatalogEcsTaskExecutionRoleF3ED238D"
     }
    ]
   }
  },
```

IAM Policy를 생성하는 리소스입니다. 이 IAM Policy는 앞서 생성한 Catalog 서비스의 Task를 실행할 때 사용되는 Task Execution Role에 연결됩니다.

---

## 7. `"NetworkECSInfraRoleVpcLattice46E30754"`

```json
  "NetworkECSInfraRoleVpcLattice46E30754": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS VPC Lattice Infra Role",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/AmazonECSInfrastructureRolePolicyForVpcLattice"
       ]
      ]
     }
    ],
    "RoleName": "ECSInfraRoleVpcLattice",
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
   }
  },
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 VPC Lattice 실습에서 VPC Lattice 연동에 필요한 인프라 작업을 수행하기 위한 IAM Role입니다.

---

`"ManagedPolicyArns"`에 지정된 `AmazonECSInfrastructureRolePolicyForVpcLattice`는 ECS가 VPC Lattice 연동에 필요한 인프라 작업을 수행할 수 있도록 하는 AWS Managed Policy입니다.

---

## 8. `"NetworkCartsEcsVpcLatticeTaskRole2E197958"`

```json
  "NetworkCartsEcsVpcLatticeTaskRole2E197958": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "ECS Carts Task Role for VPC Lattice Lab",
    "RoleName": "ECSImmesionDay_CartsVpcLatticeTaskRole",
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
   }
  },
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 VPC Lattice에 위치한 Carts 서비스의 Task가 실행 중에 사용하는 Task Role입니다.

---

## 9. `"NetworkCartsEcsVpcLatticeTaskRoleDefaultPolicy28F12718"`

```json
  "NetworkCartsEcsVpcLatticeTaskRoleDefaultPolicy28F12718": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": "dynamodb:*",
       "Effect": "Allow",
       "Resource": "*"
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "NetworkCartsEcsVpcLatticeTaskRoleDefaultPolicy28F12718",
    "Roles": [
     {
      "Ref": "NetworkCartsEcsVpcLatticeTaskRole2E197958"
     }
    ]
   }
  },
```

IAM Policy를 생성하는 리소스입니다. 이 IAM Policy는 앞서 생성한 Carts 서비스의 Task가 실행 중에 사용하는 Task Role에 연결됩니다.

---

`"Action": "dynamodb:*"`는 DynamoDB에 대한 모든 작업을 의미합니다.

`"Resource": "*"`이기 때문에, `"Action"`의 대상이 되는 리소스는 제한되지 않습니다. 즉, 특정 DynamoDB 리소스가 아니라, 모든 DynamoDB 리소스가 작업 대상이 될 수 있습니다.

---

## 10. `"NetworkECSServiceConnectCertificateRole144192CD"`

```json
  "NetworkECSServiceConnectCertificateRole144192CD": {
   "Type": "AWS::IAM::Role",
   "Properties": {
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs.amazonaws.com"
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "Description": "IAM Role for ECS Service Connect to issue certificates from AWS Private CA",
    "ManagedPolicyArns": [
     {
      "Fn::Join": [
       "",
       [
        "arn:",
        {
         "Ref": "AWS::Partition"
        },
        ":iam::aws:policy/service-role/AmazonECSInfrastructureRolePolicyForServiceConnectTransportLayerSecurity"
       ]
      ]
     }
    ],
    "RoleName": "ecs-service-connect-certificate-role",
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
   }
  }
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 ECS Service Connect가 TLS 인증서 발급에 필요한 인프라 작업을 수행하기 위한 IAM Role입니다.

---

`"ManagedPolicyArns"`에 지정된 `AmazonECSInfrastructureRolePolicyForServiceConnectTransportLayerSecurity`는 ECS Service Connect가 TLS 인증서 발급에 필요한 인프라 작업을 수행할 수 있도록 하는 AWS Managed Policy입니다.
