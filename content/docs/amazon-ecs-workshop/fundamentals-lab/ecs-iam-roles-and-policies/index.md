---
title: "06. ECS 권한 (IAM Roles & Policies)"
weight: 6
draft: true
---
> **작성일:** 2026-04-19 | **수정일:** 2026-04-19
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

IAM Role을 생성합니다. `"EcsTaskExecutionRole"`은 ECS Task 실행에 필요한 권한을 담는 IAM Role입니다.

---

IAM Role은 특정 AWS 리소스가 다른 AWS 리소스에 접근할 수 있도록 권한을 담는 스팟입니다.

---

```json
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
```

`"AssumeRolePolicyDocument"`는 "누가 이 IAM Role을 사용할 수 있는가"를 정의하는 정책입니다.

`"Statement"`는 이 정책에서 하나의 규칙을 나타내는 항목입니다. `"Action"`, `"Effect"`, `"Principal"` 등을 통해 어떤 행위를 정의하는지, 그 행위를 허용할지 거부할지, 그리고 그 행위의 주체가 누구인지를 정합니다.

`"Action"`은 이 정책에서 정의하는 행위를 의미합니다.

`"Effect"`는 해당 행위를 허용할지 거부할지를 의미합니다. 여기서는 `"Allow"`이므로 허용한다는 뜻입니다.

`"sts:AssumeRole"`에서 `"sts"`는 AWS Security Token Service를 의미하고, `"AssumeRole"`은 어떤 주체가 IAM Role을 assume(맡아 사용하는)하는 것이라고 볼 수 있습니다. 이를 통해 AWS STS는 해당 주체에게 임시 보안 자격 증명을 발급합니다. 이 임시 보안 자격 증명에는 access key ID, secret access key, session token이 포함되며, 일정 시간이 지나면 만료됩니다. 따라서 해당 주체가 IAM Role을 영구적으로 소유하는 것이 아니라, 일정 시간 동안만 그 Role에 연결된 권한을 사용할 수 있습니다. 즉, 엄밀히 말하면 IAM 권한을 소유한다기보다 잠시 빌려쓴다고 볼 수 있습니다.

`"Principal"`은 이 IAM Role을 assume할 수 있는 주체를 뜻합니다. 여기서는 `"ecs-tasks.amazonaws.com"`가 이 IAM Role을 assume할 수 있는 주체입니다.

---

```json
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
```

`"ManagedPolicyArns"`는 AWS가 미리 만들어 둔 관리형 IAM Policy를 이 IAM Role에 붙이는 부분입니다. IAM Role이 권한을 담는 스팟(주머니)이라면, IAM Policy는 실제 권한을 정의한 문서라고 볼 수 있습니다.

`"Ref": "AWS::Partition"`은 현재 CloudFormation 스택이 배포되는 AWS 파티션 값을 참조하는 부분입니다. AWS 파티션은 리전보다 상위의 구분 단위이며, 이를 사용하면 ARN을 하드코딩하지 않고 현재 환경에 맞게 생성할 수 있습니다. 일반적인 상용 AWS 리전에서는 보통 `"aws"`가 들어가며, AWS 파티션 값으로는 `"aws"`, `"aws-cn"`, `"aws-us-gov"`, `"aws-eusc"` 등이 있습니다.

`"Fn::Join"`로 여러 문자열을 이어 붙여 `"AmazonECSTaskExecutionRolePolicy"`의 ARN을 생성합니다. `"AmazonECSTaskExecutionRolePolicy"`는 AWS가 미리 만들어 둔, ECS Task 실행에 필요한 권한을 정의한 Managed Policy입니다.

---

```json
    "PermissionsBoundary": {
     "Ref": "PermissionsBoundary2D0B48CC"
    },
```

`"PermissionsBoundary"`는 이 IAM Role에 붙은 IAM Policy들이 부여할 수 있는 최대 권한 범위를 제한하는 설정입니다. 여기서는 `"PermissionsBoundary2D0B48CC"`를 참조해 그 경계선을 설정합니다.

---

`"RoleName"` - 이 권한 스팟(IAM Role)의 이름입니다.

요약하면, 이 코드는 ECS Task가 사용할 IAM Role을 만들고, 그 Role에 AWS가 미리 만들어 둔 관리형 권한 문서를 붙이며, `"PermissionsBoundary"`로 그 Role이 가질 수 있는 최대 권한 범위를 제한하는 코드입니다. `"AssumeRolePolicyDocument"`는 어떤 주체가 이 IAM Role을 사용할 수 있는지를 정하고, 여기서는 `ecs-tasks.amazonaws.com`, 즉 ECS Task가 그 대상입니다. `"ManagedPolicyArns"`는 이 Role에 붙일 실제 권한 문서를 지정하는 부분이며, `Fn::Join`과 `"AWS::Partition"`을 사용해 `"AmazonECSTaskExecutionRolePolicy"`의 ARN을 환경에 맞게 생성합니다. 마지막으로 `"PermissionsBoundary"`는 이 Role에 Policy가 붙더라도 실제로는 어디까지 권한을 가질 수 있는지를 제한하는 경계선 역할을 합니다.

- `"AWS::IAM::Role"` - 권한 스팟 자체
- `"AssumeRolePolicyDocument"` - 어떤 주체가 이 권한 스팟(IAM Role)을 사용할 수 있는지 정하는 규칙
- `"ManagedPolicyArns"` - 이 권한 스팟(IAM Role)에 붙일 실제 권한 문서(IAM Policy)들 목록
- `"PermissionsBoundary"` - 붙인 권한(IAM Policy)이 있더라도 실제로는 여기까지만 가능하다는 상한선
- `"RoleName"` - 이 권한 스팟의 이름
- `"Principal"` - 이 권한 스팟을 사용할 수 있는 주체
- `"Action": "sts:AssumeRole"` - 그 주체가 이 권한 스팟을 맡아 쓰는 동작
- `"Effect": "Allow"` - 그 동작을 허용함

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

IAM Role을 생성합니다. `EcsTaskExecutionRoleDefaultPolicy`는 ECS Task 실행에 필요한 권한을 정의한 IAM Policy입니다.

`"BasicRetailStoreEcsTaskExecutionRoleCAE88D2C"`는 IAM Role 자체입니다. 이 IAM Role에는 앞서 Managed Policy가 붙었고, 여기서 정의한 IAM Policy도 `"Roles"` 속성을 통해 같은 IAM Role에 추가로 붙습니다.

---

```json
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
```

`"Statement"`는 이 정책의 개별 규칙입니다. 

`"Action"`은 이 정책이 허용할 AWS 작업 목록입니다.

`"ecs:TagResource"`는 ECS 리소스에 태그를 붙일 수 있다는 것을 의미합니다. 리소스에 태그가 붙으면 동일한 태그를 가진 리소스를 한번에 필터링할 수 있기 때문에 리소스들의 관리가 편해집니다. 

`"logs:CreateLogGroup"`는 CloudWatch Logs의 로그 그룹을 만들 수 있음을 의미합니다. 

`"ssm:GetParameters"`는 Systems Manager Parameter Store에 저장된 파라미터 값을 읽어올 수 있음을 의미합니다. Systems Manager Parameter Store는 애플리케이션 실행에 필요한 파라미터들을 저장하는 AWS 서비스입니다.

`"Resource": "*"`는 `"Action"`의 대상이 되는 리소스를 모든 리소스로 지정한다는 뜻입니다. 즉, 특정 리소스로 대상을 제한하지 않겠다는 의미입니다.

---

```json
    "Roles": [
     {
      "Ref": "BasicRetailStoreEcsTaskExecutionRoleCAE88D2C"
     }
```

이 IAM Policy를 IAM Role에 연결합니다.

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

IAM Role을 생성합니다. `"EcsTaskRole"`은 ECS Task 안에서 실행되는 애플리케이션 컨테이너가 사용하는 IAM Role입니다. `"EcsTaskRole"`은 개별 컨테이너에 직접 부여되는 IAM Role이 아니라, ECS Task에 부여되는 IAM Role입니다. 따라서 해당 Task 안에서 실행되는 컨테이너는 이 Role의 임시 자격 증명을 사용할 수 있습니다.

---

```json
    "AssumeRolePolicyDocument": {
     "Statement": [
      {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Principal": {
        "Service": "ecs-tasks.amazonaws.com"
       }
      }
```

첫 번째 IAM Role(`"BasicRetailStoreEcsTaskExecutionRoleCAE88D2C"`)와 동일한 `"AssumeRolePolicyDocument"`를 가집니다. 즉, ECS Task가 이 IAM Role을 assume(맡아 사용)할 수 있습니다. 다만 첫 번째 IAM Role과는 다르게, 여기서는 Managed Policy를 붙이지 않습니다.

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

IAM Policy를 생성합니다. 앞선 IAM Role(`"BasicRetailStoreEcsTaskRole14589F14"`)에 추가하는 IAM Policy입니다.


