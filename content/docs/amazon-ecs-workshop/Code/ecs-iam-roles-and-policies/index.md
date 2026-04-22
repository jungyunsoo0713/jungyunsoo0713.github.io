---
title: "06. ECS 권한 (IAM Roles & Policies)"
weight: 6
---
> **작성일:** 2026-04-19 | **수정일:** 2026-04-21
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

IAM Role을 생성합니다. ECS Task Execution Role은 ECS Task 실행에 필요한 권한을 담는 IAM Role입니다.

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

`"AssumeRolePolicyDocument"`는 누가 이 IAM Role을 맡을 수 있는지를 정의하는 정책, 즉 Trust Policy를 의미합니다.

`"Statement"`는 이 정책에서 하나의 규칙을 나타내는 항목입니다. `"Action"`, `"Effect"`, `"Principal"` 등을 통해 어떤 행위를 정의하는지, 그 행위를 허용할지 거부할지, 그리고 그 행위의 주체가 누구인지를 정합니다.

`"Action"`은 이 정책에서 정의하는 행위를 의미합니다.

`"Effect"`는 해당 행위를 허용할지 거부할지를 의미합니다. 여기서는 `"Allow"`이므로 허용한다는 뜻입니다.

`"sts:AssumeRole"`에서 `"sts"`는 AWS Security Token Service를 의미하고, `"AssumeRole"`은 어떤 주체가 IAM Role을 assume, 즉 맡아 사용하는 것을 의미합니다. 이 Role이 실제로 Assume되면 AWS STS는 해당 Role에 대한 임시 보안 자격 증명을 발급합니다. 이 임시 보안 자격 증명에는 Access Key ID, Secret Access Key, Session Token이 포함되며, 일정 시간이 지나면 만료됩니다. 따라서 해당 주체가 IAM Role 자체를 영구적으로 소유하는 것이 아니라, 일정 시간 동안만 그 Role에 연결된 권한을 임시로 사용할 수 있습니다. 즉, 엄밀히 말하면 IAM 권한을 소유한다기보다 잠시 빌려쓴다고 볼 수 있습니다.

`"Principal"`은 이 IAM Role을 assume할 수 있는 주체를 뜻합니다. 여기서는 `ecs-tasks.amazonaws.com`가 이 IAM Role을 assume할 수 있는 주체입니다.

즉, `"Statement"`는 `ecs-tasks.amazonaws.com` 서비스가 이 IAM Role을 맡을 수 있도록 허용하며, 그 결과 AWS STS를 통해 해당 Role의 임시 자격 증명이 발급될 수 있음을 의미합니다.
임시 자격 증명은 해당 IAM Role을 assume한 주체가 받아서 사용합니다. Execution Role의 경우 ECS가 Task 실행 과정에서 이를 사용하고, Task Role의 경우 보통 컨테이너 내부 애플리케이션이 이를 사용합니다.

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

`"Ref": "AWS::Partition"`은 현재 CloudFormation 스택이 배포되는 AWS 파티션 값을 참조하는 부분입니다. AWS 파티션은 리전보다 상위의 구분 단위이며, 이를 사용하면 ARN을 하드코딩하지 않고 현재 환경에 맞게 생성할 수 있습니다. 일반적인 상용 AWS 리전에서는 보통 `"aws"`가 들어가며, AWS 파티션 값으로는 `"aws"`, `"aws-cn"`, `"aws-us-gov"` 등이 있습니다.

`"Fn::Join"`로 여러 문자열을 이어 붙여 `"AmazonECSTaskExecutionRolePolicy"`의 ARN을 생성합니다. `"AmazonECSTaskExecutionRolePolicy"`는 AWS가 미리 만들어 둔, ECS Task 실행 과정에서 ECS/Fargate가 사용자 대신 필요한 AWS API를 호출할 수 있게 하는 권한을 정의한 Managed Policy입니다. 주로 ECS/Fargate가 사용자 대신 이미지 풀이나 로그 전송 등 실행 과정에서 AWS API를 호출할 때 사용하는 권한입니다.

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

IAM Policy를 생성합니다. `"EcsTaskExecutionRoleDefaultPolicy"`는 ECS Task 실행에 필요한 권한을 정의한 IAM Policy입니다.

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

`"ecs:TagResource"`는 ECS 리소스에 태그를 붙일 수 있다는 것을 의미합니다. 리소스에 태그가 붙으면 동일한 태그를 가진 리소스를 한 번에 필터링할 수 있기 때문에 리소스들의 관리가 편해집니다. 

`"logs:CreateLogGroup"`는 CloudWatch Logs의 로그 그룹을 만들 수 있음을 의미합니다. 

`"ssm"`은 AWS Systems Manager를 의미합니다.

AWS Systems Manager는 관리 대상인 EC2 인스턴스나 다른 리소스들을 중앙에서 관리하는 서비스입니다. 이 글에서는 ECS Task와 관련된 권한을 설명하고 있으므로, Systems Manager가 컨테이너와 연결되거나 명령 전달에 관여하는 상황도 함께 등장할 수 있습니다. 다만 Systems Manager가 컨테이너를 직접 대신 실행하는 것은 아니며, 실제 명령 실행은 컨테이너 내부에서 이루어집니다. 사용자가 입력한 명령은 Systems Manager가 받아 Systems Manager의 세션 채널을 통해 컨테이너로 전달하고, 실제 명령 실행은 컨테이너 내부에서 이루어집니다.

`"ssm:GetParameters"`는 Systems Manager의 Parameter Store 기능에 저장된 파라미터 값을 읽어올 수 있음을 의미합니다. ECS Task 실행 과정에서 task definition이 Parameter Store의 값을 참조하는 경우 이 권한이 필요할 수 있습니다.

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

IAM Role을 생성합니다. ECS Task Role은 ECS Task 안에서 실행되는 애플리케이션 컨테이너가 사용하는 IAM Role입니다. ECS Task Role은 개별 컨테이너에 직접 부여되는 IAM Role이 아니라, ECS Task에 부여되는 IAM Role입니다. 따라서 해당 Task 안에서 실행되는 컨테이너는 이 Role의 임시 자격 증명을 사용할 수 있습니다.

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

첫 번째 IAM Role(`"BasicRetailStoreEcsTaskExecutionRoleCAE88D2C"`)와 동일한 `"AssumeRolePolicyDocument"`를 가집니다. 즉, ECS Task가 이 IAM Role을 assume(맡아 사용)할 수 있습니다. 다만 첫 번째 IAM Role은 ECS Task 실행 과정에서 ECS가 사용하는 Execution Role이고, 이 IAM Role은 Task 안에서 실행되는 애플리케이션 컨테이너가 사용하는 Task Role이라는 차이가 있습니다.

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

---

```json
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
```

이 `"PolicyDocument"`에 포함된 첫 번째 권한은 ECS Task 안의 컨테이너가 AWS Systems Manager와 통신하는 데 필요한 `"ssmmessages"` 관련 작업을 허용합니다.

AWS Systems Manager에는 Parameter Store 외에도 Session Manager 기능이 있습니다.

`"ssmmessages"`는 AWS Systems Manager의 세션 통신에서 메시지/채널 연결에 사용되는 API 네임스페이스입니다. `"CreateControlChannel"`에서 `"ControlChannel"`은 명령과 제어 신호가 오가는 제어 채널을 의미합니다. 따라서 `"ssmmessages:CreateControlChannel"`은 ECS 태스크 내부의 컨테이너가 Systems Manager와 제어 메시지를 주고받기 위한 제어 채널을 생성할 수 있음을 의미합니다.

`"ssmmessages:CreateDataChannel"`는 데이터 메시지를 위한 데이터 채널을 생성할 수 있음을 의미합니다.  

제어 메시지와 데이터 메시지의 차이는, 제어 메시지는 연결과 실행을 지시하는 메시지고, 데이터 메시지는 그 연결과 실행 과정에서 입력값, 출력값, 실행 결과처럼 실제로 전달되는 내용을 담은 메시지입니다. 

예를 들면, 사용자가 ECS 안의 컨테이너에 접속해 `ls` 명령어를 입력하면, Systems Manager가 이를 컨테이너로 전달하는 상황을 생각해볼 수 있습니다. 이때 “세션을 열어라”, “채널을 만들어라”, “`ls`를 실행해라” 같은 지시는 제어 메시지에 해당합니다. 반면 `ls` 실행 후 화면에 출력된 파일 목록, 입력한 명령어, 명령 실행 결과로 전달되는 실제 출력 내용은 데이터 메시지에 해당합니다.

`"ssmmessages:OpenControlChannel"`은 컨테이너가 Systems Manager와 제어 메시지를 주고받을 수 있도록 하는 제어 채널을 실제로 여는 권한입니다. 앞서 `"ssmmessages:CreateControlChannel"`이 제어 채널을 생성하는 권한이라면, `"ssmmessages:OpenControlChannel"`은 이렇게 생성된 채널을 실제 연결 상태로 여는 권한입니다.

`"ssmmessages:OpenDataChannel"`은 `"ssmmessages:CreateDataChannel"`로 생성한 데이터 채널을 실제로 열어, Systems Manager와 데이터 입출력을 주고받을 수 있게 하는 권한입니다.

정리하자면:

- `"ssmmessages:CreateControlChannel"` → 제어 채널 생성
- `"ssmmessages:OpenControlChannel"` → 생성된 제어 채널 열기
- `"ssmmessages:CreateDataChannel"` → 데이터 채널 생성
- `"ssmmessages:OpenDataChannel"` → 생성된 데이터 채널 열기

---

`"Resource"`의 값에는 보통 특정 AWS 리소스의 ARN처럼, 권한의 대상을 식별하는 값이 들어갑니다. 하지만 이 경우에는 특정 리소스 하나를 지정하는 방식으로 권한 대상을 표현하는 것이 적절하지 않기 때문에 `"Resource": "*"`로 설정합니다. 

IAM에서는 권한을 지정할 때 무엇을 할 수 있는지와 어떤 대상을 상대로 할 수 있는지를 나누어 설정합니다. 여기서 `"ssmmessages:CreateControlChannel"`, `"ssmmessages:CreateDataChannel"`, `"ssmmessages:OpenControlChannel"`, `"ssmmessages:OpenDataChannel"`은 특정 리소스를 뜻하는 것이 아니라, 허용할 작업을 나타내는 `"Action"`입니다. IAM에서는 어떤 작업이 특정 리소스 ARN 단위로 권한 대상을 지정할 수 없는 경우 `"Resource"`에 `"*"`를 사용합니다. 여기서 사용된 `"ssmmessages"` 관련 작업들은 리소스 수준 권한 지정을 지원하지 않기 때문에 `"Resource": "*"`로 설정합니다.

---

```json
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
```

이 `"PolicyDocument"`에 포함된 두 번째 권한은 ECS Task 안의 컨테이너가 특정 EFS 파일 시스템을 마운트하고 사용할 수 있도록 허용합니다.

---

```json
       "Action": [
        "elasticfilesystem:ClientMount",
        "elasticfilesystem:ClientRootAccess",
        "elasticfilesystem:ClientWrite"
       ],
```

`"elasticfilesystem:ClientMount"`는 EFS 파일 시스템을 마운트할 수 있음을, `"elasticfilesystem:ClientRootAccess"`는 EFS에 root 사용자로 접근할 수 있음을, `"elasticfilesystem:ClientWrite"`는 파일을 쓸 수 있음을 의미합니다.

---

```json
       "Condition": {
        "Bool": {
         "elasticfilesystem:AccessedViaMountTarget": "true"
        }
       },
```

`"Condition"`은 이 권한이 적용되는 조건을 의미합니다. 여기서는 `"elasticfilesystem:AccessedViaMountTarget": "true"`로 설정되어 있으므로, 해당 EFS 파일 시스템에 대한 접근은 EFS Mount Target을 통한 정상적인 마운트 방식일 때만 허용됩니다. 이 마운트 방식은 EFS를 네트워크 드라이브처럼 직접 붙여서 사용하는 기본 방식입니다. 참고로 EFS에 접근할 때는 정해진 경로와 사용자 규칙을 강제로 적용하는 전용 진입점인 EFS Access Point를 통한 마운트 방식을 사용할 수도 있으며, AWS Transfer Family 서비스를 통해 접근하는 방식도 존재합니다.

---

```json
       "Effect": "Allow",
       "Resource": {
        "Fn::GetAtt": [
         "StorageEfsFileSystemF296E1B3",
         "Arn"
        ]
       }
```

`"Resource"`에는 `"Fn::GetAtt"`를 사용해 `"StorageEfsFileSystemF296E1B3"` 리소스의 `"Arn"` 값을 가져오고 있습니다. 즉, 이 권한은 모든 EFS가 아니라 해당 EFS 파일 시스템 하나에만 적용됩니다.
