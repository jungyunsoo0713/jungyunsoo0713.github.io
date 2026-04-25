---
title: "05. 권한 관리 (IAM Roles & Policies)"
weight: 5
---
> **작성일:** 2026-04-25 | **수정일:** 2026-04-25
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

Orders 서비스를 위한 ECS Task 실행에 필요한 권한을 담는 IAM Role을 생성합니다. 

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

Orders 서비스의 Task Execution Role에 추가 권한을 부여하기 위한 IAM Policy를 생성합니다.

---

```json
       "Action": [
        "kms:Decrypt",
        "secretsmanager:GetSecretValue",
        "ssm:GetParameters"
       ],
```

이 IAM Policy는 KMS Key로 암호화된 값을 복호화하는 권한(`"kms:Decrypt"`), Secrets Manager에 저장된 Secret 값을 읽는 권한(`"secretsmanager:GetSecretValue"`), AWS Systems Manager Parameter Store에 저장된 값을 읽는 권한(`"ssm:GetParameters"`)을 허용합니다.

---

```json
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
```

`"Resource"`는 위 `"Action"`들이 적용될 대상 리소스 목록으로, 여기서는 3개 리소스를 대상으로 권한을 허용하고 있습니다. 각각 KMS Key인 `"NetworkKMSKey37A7DAB2"`, DB 엔드포인트 값을 저장한 SSM Parameter인 `"NetworkDBEndpointParameter9BA21FDB"`, DB 접속 정보를 저장한 Secrets Manager Secret인 `"NetworkDBCredentialsSecretADE5ABB3"`입니다.

이 중 SSM Parameter는 IAM Policy의 `"Resource"`에 넣을 때 ARN 형식으로 표현해야 합니다. 그런데 `"NetworkDBEndpointParameter9BA21FDB"`에 `Ref`를 사용하면 SSM Parameter 리소스의 이름이 반환됩니다. 따라서 SSM Parameter의 이름만으로는 IAM Policy의 대상 리소스를 정확한 ARN으로 표현하기 부족하므로, 현재 AWS 계정과 리전에 맞는 SSM Parameter ARN을 직접 조립하기 위해 `Fn::Join`을 사용합니다.

DB 엔드포인트 값은 Orders 애플리케이션이 Aurora PostgreSQL DB에 접속하기 위한 주소와 포트 정보입니다. DB 계정과 비밀번호는 Secrets Manager에서 가져오고, DB가 어디에 있는지는 SSM Parameter Store에 저장된 DB 엔드포인트 값을 통해 가져옵니다.

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

Checkout 서비스를 위한 ECS Task 실행에 필요한 권한을 담는 IAM Role을 생성합니다. `"NetworkOrdersEcsTaskExecutionRole0CFAB02D"`와 동일한 구조를 가지고 있습니다

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

Checkout 서비스의 Task Execution Role에 추가 권한을 부여하기 위한 IAM Policy를 생성합니다.

---

`"NetworkOrdersEcsTaskExecutionRoleDefaultPolicy1D6EAD89"` 정책 리소스와 거의 같은 구조를 가지고 있습니다.

다만 Orders 서비스는 Aurora PostgreSQL DB 접속을 위해 SSM Parameter의 DB 엔드포인트와 Secrets Manager의 DB 계정 정보를 함께 읽어야 하므로 `"secretsmanager:GetSecretValue"` 권한이 포함되어 있습니다. 반면 Checkout 서비스는 Redis 엔드포인트 값을 SSM Parameter Store에서 읽으면 되기 때문에 `"kms:Decrypt"`와 `"ssm:GetParameters"` 권한만 포함되어 있습니다.

Checkout Redis 엔드포인트는 서비스 접속 위치 정보라서 관리 대상이지만, Orders DB Credential처럼 직접 인증에 쓰이는 비밀번호급 민감 정보는 아닙니다. 이 템플릿에서는 Redis 접근을 Secret 기반 인증보다는 VPC 내부 네트워크와 Security Group으로 제한하는 구조로 볼 수 있습니다.

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

Catalog 서비스를 위한 ECS Task 실행에 필요한 권한을 담는 IAM Role을 생성합니다. `"NetworkOrdersEcsTaskExecutionRole0CFAB02D"`와 동일한 구조를 가지고 있습니다

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

Catalog 서비스의 Task Execution Role에 추가 권한을 부여하기 위한 IAM Policy를 생성합니다. `"NetworkOrdersEcsTaskExecutionRoleDefaultPolicy1D6EAD89"`와 동일한 구조를 가지고있습니다.

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


ECS 서비스들을 VPC Lattice와 연동하기 위한 인프라용 IAM Role을 생성합니다. 이는 ECS가 VPC Lattice 관련 리소스를 연결하고 관리할 때 사용하는 IAM Role입니다.

ECS 서비스가 VPC Lattice를 통해 트래픽을 받으려면, 실행 중인 ECS 태스크들이 VPC Lattice Target Group에 등록되어 있어야 합니다. 따라서 ECS는 태스크가 생성되거나 종료될 때 해당 태스크를 Target Group에 등록하거나 제거해야 합니다. 이 Role은 ECS가 이러한 VPC Lattice 연동 작업을 수행할 수 있도록 필요한 권한을 제공합니다.

VPC Lattice는 VPC 내부에 직접 배치되는 EC2나 ENI 같은 리소스라기보다는, VPC와 연결되어 서비스 간 통신을 중개하는 AWS의 애플리케이션 네트워킹 서비스입니다. VPC Lattice의 Service Network에 VPC와 서비스를 연결하면, 해당 VPC의 리소스들은 Service Network에 연결된 다른 서비스와 통신할 수 있습니다.

---

`"ManagedPolicyArns"`에는 AWS Managed Policy인
`"AmazonECSInfrastructureRolePolicyForVpcLattice"` 가 연결됩니다. ECS는 이 정책을 통해 태스크가 어느 VPC와 서브넷에 있는지 같은 인프라 정보를 조회하고, 실행 중인 태스크를 VPC Lattice Target Group에 등록하거나 종료된 태스크를 제거할 수 있습니다.

즉, 이 정책은 EC2 인스턴스 내부의 파일, 애플리케이션 데이터, 로그를 읽거나 EC2 인스턴스/VPC/서브넷을 생성·삭제하는 정책이 아닙니다. 대신 ECS가 “이 태스크가 어느 VPC에 있지?”, “어느 서브넷에 있지?”, “어떤 네트워크 환경에 붙어 있지?”와 같은 인프라 위치 정보를 확인하고, 실행 중인 ECS 태스크를 VPC Lattice Target Group에 등록하거나 제거하기 위한 정책입니다.

참고로 VPC Lattice도 ALB와 비슷하게 Target Group을 사용합니다. 요청을 받은 뒤 Target Group에 등록된 실제 대상, 예를 들어 ECS 태스크로 트래픽을 전달한다는 점에서는 ALB와 유사합니다. Target Group이 필요한 이유는 VPC Lattice가 요청을 전달할 실제 실행 대상을 알아야 하기 때문입니다.

ECS 태스크는 생성되고 종료되면서 IP가 바뀔 수 있으므로, ECS는 실행 중인 태스크를 VPC Lattice Target Group에 등록하고 종료된 태스크는 제거합니다. 다만 VPC Lattice는 단순한 로드 밸런서라기보다, 여러 VPC와 서비스 사이의 통신을 서비스 단위로 연결하고 관리하는 네트워킹 계층에 가깝습니다.

> `BasicRetailStoreVPC0FA21CD5`를 VPC1이라고 하고, `NetworkRetailStoreVPC2A069619D`를 VPC2라고 보면, `RDS`, `Redis`, `Orders`, `Checkout`, `Catalog` 같은 기존 주요 애플리케이션 및 데이터 리소스는 VPC1에 배치됩니다.

>반면 VPC2에는 VPC Lattice 실습을 위해 별도로 둔 `Carts` 서비스가 배치됩니다. 즉, 이 구조는 VPC1에 기존 쇼핑몰 서비스들을 두고, VPC2에 `Carts` 서비스를 따로 둔 다음, VPC Lattice를 통해 VPC1의 서비스들이 VPC2의 `Carts` 서비스를 서비스 이름 기반으로 호출할 수 있게 하는 구조로 이해하면 됩니다.

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

VPC Lattice Lab에서 사용하는 `Carts` 서비스의 ECS Task Role을 생성합니다. 이 Role은 ECS 인프라 관리용 Role이 아니라, `Carts` 태스크 안에서 실행되는 애플리케이션 컨테이너가 사용할 수 있는 Role입니다.

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

VPC Lattice Lab에서 사용하는 `Carts` 서비스의 ECS Task Role에 추가될 IAM Policy를 생성합니다.

---

`"Action": "dynamodb:*"`는 DynamoDB에 대한 모든 작업을 허용한다는 의미입니다. 즉, `Carts` 애플리케이션 컨테이너는 이 Role을 통해 DynamoDB 테이블을 생성, 조회, 수정, 삭제하는 등 DynamoDB 관련 작업을 수행할 수 있습니다.

---

`"Resource": "*"`는 이 권한이 특정 DynamoDB 테이블 하나에만 제한되지 않고, 모든 DynamoDB 리소스를 대상으로 적용된다는 뜻입니다.

---

`"Roles"`에는 이 정책이 연결될 IAM Role이 지정됩니다. 여기서는 `NetworkCartsEcsVpcLatticeTaskRole2E197958`에 연결되므로, VPC Lattice Lab의 `Carts` 태스크가 DynamoDB를 사용할 수 있게 됩니다.

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

ECS Service Connect에서 TLS 인증서를 발급받기 위해 사용하는 IAM Role을 생성합니다. 

ECS Service Connect는 ECS Service끼리 서비스 이름 기반으로 통신할 수 있게 해주는 기능입니다. 이 템플릿에서는 Service Connect 통신에 TLS를 적용하기 위해 AWS Private CA에서 인증서를 발급받으며, 이 Role은 ECS가 해당 인증서 발급과 TLS 설정을 관리할 수 있도록 사용됩니다.

---

`"ManagedPolicyArns"`에는 AWS Managed Policy인 `"AmazonECSInfrastructureRolePolicyForServiceConnectTransportLayerSecurity"`가 연결됩니다.

이 정책은 ECS가 Service Connect 통신에 TLS를 적용할 수 있도록 AWS Private CA와 Secrets Manager에 대한 제한된 권한을 제공합니다. 구체적으로 ECS는 AWS Private CA에서 인증서 발급에 필요한 CA 정보를 조회하고(읽기 권한), 인증서를 발급받을 수 있습니다(쓰기 권한).

또한 Secrets Manager에서는 TLS 인증서와 관련된 비밀 값을 저장하거나(쓰기 권한), 읽고(읽기 권한), 관리용 태그를 붙일 수 있습니다(태깅 권한). ECS는 이 권한을 사용해 Service Connect TLS에 필요한 인증서 발급, 저장, 조회 작업을 사용자를 대신해 수행합니다.
