---
title: "05. 권한 관리 (IAM Roles & Policies)"
weight: 5
draft: true
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
```
