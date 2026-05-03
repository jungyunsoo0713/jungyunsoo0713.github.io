---
title: "06. ECS 권한 (IAM Roles & Policies)"
weight: 6
---
> **작성일:** 2026-05-04 | **수정일:** 2026-05-04
```json
{
  "BasicRetailStoreEcsTaskExecutionRoleCAE88D2C": {
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
    "Description": "ECS Default Task Execution Role",
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
    "PermissionsBoundary": {
     "Ref": "PermissionsBoundary2D0B48CC"
    },
    "RoleName": "retailStoreEcsTaskExecutionRole",
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
    ]
   }
  },
  "BasicRetailStoreEcsTaskExecutionRoleDefaultPolicy6D9AFE99": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "ecs:TagResource",
        "logs:CreateLogGroup",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": "*"
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "BasicRetailStoreEcsTaskExecutionRoleDefaultPolicy6D9AFE99",
    "Roles": [
     {
      "Ref": "BasicRetailStoreEcsTaskExecutionRoleCAE88D2C"
     }
    ]
   }
  },
  "BasicRetailStoreEcsTaskRole14589F14": {
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
    "Description": "ECS Default Task Role",
    "PermissionsBoundary": {
     "Ref": "PermissionsBoundary2D0B48CC"
    },
    "RoleName": "retailStoreEcsTaskRole",
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
    ]
   }
  },
  "BasicRetailStoreEcsTaskRoleDefaultPolicy9759E157": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "ssmmessages:CreateControlChannel",
        "ssmmessages:CreateDataChannel",
        "ssmmessages:OpenControlChannel",
        "ssmmessages:OpenDataChannel"
       ],
       "Effect": "Allow",
       "Resource": "*"
      },
      {
       "Action": [
        "elasticfilesystem:ClientMount",
        "elasticfilesystem:ClientRootAccess",
        "elasticfilesystem:ClientWrite"
       ],
       "Condition": {
        "Bool": {
         "elasticfilesystem:AccessedViaMountTarget": "true"
        }
       },
       "Effect": "Allow",
       "Resource": {
        "Fn::GetAtt": [
         "StorageEfsFileSystemF296E1B3",
         "Arn"
        ]
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "BasicRetailStoreEcsTaskRoleDefaultPolicy9759E157",
    "Roles": [
     {
      "Ref": "BasicRetailStoreEcsTaskRole14589F14"
     }
    ]
   }
  }
}
```

---

## 1. `"BasicRetailStoreEcsTaskExecutionRoleCAE88D2C"`

```json
  "BasicRetailStoreEcsTaskExecutionRoleCAE88D2C": {
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
    "Description": "ECS Default Task Execution Role",
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
    "PermissionsBoundary": {
     "Ref": "PermissionsBoundary2D0B48CC"
    },
    "RoleName": "retailStoreEcsTaskExecutionRole",
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
    ]
   }
  },
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 ECS Task를 실행할 때 필요한 Task Execution Role입니다. 

주의할 점으로, IAM Role은 권한 자체가 아닌 권한을 담는 상자 같은 것입니다. IAM Role 자체는 어떠한 권한도 가지고 있지 않습니다. IAM Policy를 IAM Role에 연결함으로써 권한이 부여됩니다.

---

`"AssumeRolePolicyDocument"`는 이 IAM Role의 Trust Policy를 정의합니다. Trust Policy는 이 IAM Role을 사용하는 주체를 정하는 정책입니다.

---

`"Statement"`는 규칙을 정의합니다. 여기서는 Trust Policy의 규칙을 의미합니다.

`"Action"`은 어떤 행위를 할 것인지를 정합니다. `"sts:AssumeRole"`은 이 IAM Role을 assume(맡아 사용)하는 행위를 의미합니다. 여기서 `sts`는 AWS Security Token Service(STS)를 의미합니다. IAM Role을 사용하는 주체(`"Principal"`)가 STS의 `AssumeRole` API를 호출하면, STS는 임시 보안 자격 증명을 발급합니다. 이 자격 증명에는 Access Key ID, Secret Access Key, Session Token이 포함되며, 일정 시간이 지나면 만료됩니다.

`"Effect": "Allow"`는 이 `"Action"`을 허용한다는 것을 의미합니다.

`"Principal"`은 이 IAM Role을 사용할 주체를 의미합니다. 필드값인 `"ecs-tasks.amazonaws.com"`은 ECS Task 서비스 주체를 의미합니다. ECS Task 서비스 주체는 개별 ECS Task를 말하는 것이 아닌 AWS가 제공하는 ECS Task 서비스 주체를 말합니다.

즉 이 Trust Policy의 규칙을 요약하자면, ECS Task가 이 IAM Role을 맡아 사용하는 것을 허용한다는 것입니다.

---

`"Version"`은 IAM Policy의 문법 버전을 의미합니다. 이 필드의 값인 `"2012-10-17"`는 이 정책이 만들어진 날짜를 의미하는 것이 아니라, 이 정책에서 사용되는 IAM Policy의 문법이 만들어진 날짜를 의미합니다. IAM Policy 문법 버전은 `2012-10-17`에 만들어진 버전과 `2008-10-17`에 만들어진 버전 두 개가 존재합니다. 오래된 문법 버전이 적용된 IAM Policy는 가급적 사용하지 않는 것이 좋습니다.

---

`"Description": "ECS Default Task Execution Role"`는 이 IAM Role에 대해 설명합니다. ECS Task를 실행하는 데 필요한 IAM Role이라는 설명입니다.

---

`"ManagedPolicyArns"`에서 Managed Policy는 AWS가 이미 만들어 두고 관리하는 IAM Policy(AWS Managed Policy)나 사용자가 직접 만들고 관리하는 IAM Policy(Customer Managed Policy)를 의미합니다. `"Fn::Join"` CloudFormation 함수로 이 AWS Managed Policy인 `AmazonECSTaskExecutionRolePolicy`의 ARN을 만듭니다.

참고로 `"AWS::Partition"`, 즉 AWS 파티션은 리전보다 큰 AWS 환경의 단위를 말합니다. 이 값으로는 `"aws"`, `"aws-cn"`, `"aws-us-gov"` 등이 있습니다. 특수한 경우가 아닌 한 일반적으로 이 값은 `"aws"`에 해당합니다.

---

`"PermissionsBoundary"`는 이 IAM Role이 가질 수 있는 최대 권한을 설정합니다. IAM Role은 IAM Policy에 명시된 권한을 가지는데, 이 Permissions Boundary는 IAM Policy에 명시된 권한이 있더라도 이 권한의 일부를 행사하지 못하게 할 수 있습니다. 여기서는 `"PermissionsBoundary2D0B48CC"`를 참조하여 Permissions Boundary로 연결합니다. 참고로 Permissions Boundary는 Managed Policy를 사용합니다. 따라서 여기서 Permissions Boundary로 사용되는 `"PermissionsBoundary2D0B48CC"`도 본질적으로 IAM Policy입니다.

---

`"RoleName"`은 이 IAM Role의 이름을 지정합니다. `"retailStoreEcsTaskExecutionRole"`이 이름으로 지정됩니다.

---

## 2. `"BasicRetailStoreEcsTaskExecutionRoleDefaultPolicy6D9AFE99"`

```json
  "BasicRetailStoreEcsTaskExecutionRoleDefaultPolicy6D9AFE99": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "ecs:TagResource",
        "logs:CreateLogGroup",
        "ssm:GetParameters"
       ],
       "Effect": "Allow",
       "Resource": "*"
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "BasicRetailStoreEcsTaskExecutionRoleDefaultPolicy6D9AFE99",
    "Roles": [
     {
      "Ref": "BasicRetailStoreEcsTaskExecutionRoleCAE88D2C"
     }
    ]
   }
  },
```

IAM Policy를 생성하는 리소스입니다. 앞서 생성한 IAM Role에는 `AmazonECSTaskExecutionRolePolicy` AWS Managed Policy와 `PermissionsBoundary2D0B48CC` Permissions Boundary가 지정되었습니다. 이 IAM Policy 또한 앞서 생성한 IAM Role에 연결되어 추가 권한을 부여합니다.

---

`"PolicyDocument"`는 이 IAM Policy의 정책 내용을 담는 문서입니다.

---

`"Statement"`는 이 IAM Policy의 규칙을 정의합니다.

`"Action"`은 어떤 행위를 할 것인지를 정합니다. `"ecs:TagResource"`는 ECS에 태그를 붙이는 것을 의미합니다. `"logs:CreateLogGroup"`는 로그 그룹을 생성함을 의미합니다. `"ssm:GetParameters"`는 AWS Systems Manager의 파라미터 스토어에서 파라미터 값을 가져옴을 의미합니다. 

참고로 AWS Systems Manager는 AWS의 리소스를 중앙에서 관리 및 운영할 수 있게 해주는 서비스입니다. 이 서비스의 Parameter Store 기능은 애플리케이션이나 인프라에 필요한 파라미터를 저장하는 저장소 역할을 합니다. 예를 들면, 애플리케이션 설정값이나 DB 엔드포인트, 환경 변수로 사용할 값 등을 저장합니다.

`"Effect": "Allow"` 이기 때문에, `"Action"`에 정의된 행위는 모두 허용됩니다.

`"Resource": "*"`는 특정 리소스가 아닌 위 `"Action"`이 취해질 수 있는 모든 리소스를 대상으로 한다는 의미입니다.

---

`"PolicyName"`은 AWS IAM에 생성되는 Policy 이름을 지정합니다. 여기서는 이 IAM Policy 리소스의 Logical ID(`"BasicRetailStoreEcsTaskExecutionRoleDefaultPolicy6D9AFE99"`)와 동일하게 지정합니다. AWS CDK에서 생성된 CloudFormation 템플릿의 IAM Policy 이름은 자주 이런 방식으로 지정됩니다.

---

`"Roles"`는 이 IAM Policy가 연결될 IAM Role을 지정합니다. 앞서 생성한 IAM Role을 지정해 연결합니다.

---

## 3. `"BasicRetailStoreEcsTaskRole14589F14"`

```json
  "BasicRetailStoreEcsTaskRole14589F14": {
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
    "Description": "ECS Default Task Role",
    "PermissionsBoundary": {
     "Ref": "PermissionsBoundary2D0B48CC"
    },
    "RoleName": "retailStoreEcsTaskRole",
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
    ]
   }
  },
```

IAM Role을 생성하는 리소스입니다. 이 IAM Role은 ECS Task가 실행 중에 사용하는 Task Role입니다. 이 IAM Role은 Managed Policy가 연결되어 있지 않다는 점만 제외하면, ECS Task Execution Role과 구조는 같습니다.

`"PermissionsBoundary"`를 보면 앞서 생성한 ECS Task Execution Role과 같은 Permissions Boundary를 사용함을 알 수 있습니다. 이 Permissions Boundary는 워크샵에서 생성되는 IAM Role들의 최대 권한 범위를 공통으로 제한하기 위해 사용됨을 알 수 있습니다.

---

## 4. `"BasicRetailStoreEcsTaskRoleDefaultPolicy9759E157"`

```json
  "BasicRetailStoreEcsTaskRoleDefaultPolicy9759E157": {
   "Type": "AWS::IAM::Policy",
   "Properties": {
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "ssmmessages:CreateControlChannel",
        "ssmmessages:CreateDataChannel",
        "ssmmessages:OpenControlChannel",
        "ssmmessages:OpenDataChannel"
       ],
       "Effect": "Allow",
       "Resource": "*"
      },
      {
       "Action": [
        "elasticfilesystem:ClientMount",
        "elasticfilesystem:ClientRootAccess",
        "elasticfilesystem:ClientWrite"
       ],
       "Condition": {
        "Bool": {
         "elasticfilesystem:AccessedViaMountTarget": "true"
        }
       },
       "Effect": "Allow",
       "Resource": {
        "Fn::GetAtt": [
         "StorageEfsFileSystemF296E1B3",
         "Arn"
        ]
       }
      }
     ],
     "Version": "2012-10-17"
    },
    "PolicyName": "BasicRetailStoreEcsTaskRoleDefaultPolicy9759E157",
    "Roles": [
     {
      "Ref": "BasicRetailStoreEcsTaskRole14589F14"
     }
    ]
   }
  }
```

IAM Policy를 생성하는 리소스입니다. 앞서 생성한 IAM Role(`"BasicRetailStoreEcsTaskRole14589F14"`)에는 `PermissionsBoundary2D0B48CC` Permissions Boundary가 지정되었습니다. 이 IAM Policy 또한 연결되어 앞서 생성한 IAM Role에 실제 권한을 부여합니다.

---

