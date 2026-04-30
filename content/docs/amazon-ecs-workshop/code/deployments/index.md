---
title: "Deployments"
weight: 1
---
> **작성일:** 2026-04-28 | **수정일:** 2026-04-28
```json
{
  "DeploymentsManualApprovalBucket4C81F2B1": {
    "Type": "AWS::S3::Bucket",
    "Properties": {
      "BucketEncryption": {
        "ServerSideEncryptionConfiguration": [
          {
            "ServerSideEncryptionByDefault": {
              "SSEAlgorithm": "AES256"
            }
          }
        ]
      },
      "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "BlockPublicPolicy": true,
        "IgnorePublicAcls": true,
        "RestrictPublicBuckets": true
      },
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
  },
  "DeploymentsManualApprovalBucketPolicyA1F410F9": {
    "Type": "AWS::S3::BucketPolicy",
    "Properties": {
      "Bucket": {
        "Ref": "DeploymentsManualApprovalBucket4C81F2B1"
      },
      "PolicyDocument": {
        "Statement": [
          {
            "Action": "s3:*",
            "Condition": {
              "Bool": {
                "aws:SecureTransport": "false"
              }
            },
            "Effect": "Deny",
            "Principal": {
              "AWS": "*"
            },
            "Resource": [
              {
                "Fn::GetAtt": [
                  "DeploymentsManualApprovalBucket4C81F2B1",
                  "Arn"
                ]
              },
              {
                "Fn::Join": [
                  "",
                  [
                    {
                      "Fn::GetAtt": [
                        "DeploymentsManualApprovalBucket4C81F2B1",
                        "Arn"
                      ]
                    },
                    "/*"
                  ]
                ]
              }
            ]
          }
        ],
        "Version": "2012-10-17"
      }
    }
  },
  "DeploymentsECSInfrastructureRole1FA52583": {
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
      "Description": "ECS Infrastructure role for managing load balancer resources during blue-green deployments",
      "ManagedPolicyArns": [
        {
          "Fn::Join": [
            "",
            [
              "arn:",
              {
                "Ref": "AWS::Partition"
              },
              ":iam::aws:policy/AmazonECSInfrastructureRolePolicyForLoadBalancers"
            ]
          ]
        }
      ],
      "RoleName": "ecsInfrastructureRoleForLoadBalancers",
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
  "DeploymentsECSLifecycleHookRole2D97E826": {
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
      "Description": "Role for ECS to invoke Lambda functions during lifecycle hooks",
      "RoleName": "ECSLifecycleHookRole",
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
  "DeploymentsECSLifecycleHookRoleDefaultPolicyFB30EB40": {
    "Type": "AWS::IAM::Policy",
    "Properties": {
      "PolicyDocument": {
        "Statement": [
          {
            "Action": "lambda:InvokeFunction",
            "Effect": "Allow",
            "Resource": {
              "Fn::GetAtt": [
                "DeploymentsManualApprovalLambdaAAA9D0C2",
                "Arn"
              ]
            }
          }
        ],
        "Version": "2012-10-17"
      },
      "PolicyName": "DeploymentsECSLifecycleHookRoleDefaultPolicyFB30EB40",
      "Roles": [
        {
          "Ref": "DeploymentsECSLifecycleHookRole2D97E826"
        }
      ]
    }
  },
  "DeploymentsManualApprovalLambdaRole539C07A1": {
    "Type": "AWS::IAM::Role",
    "Properties": {
      "AssumeRolePolicyDocument": {
        "Statement": [
          {
            "Action": "sts:AssumeRole",
            "Effect": "Allow",
            "Principal": {
              "Service": "lambda.amazonaws.com"
            }
          }
        ],
        "Version": "2012-10-17"
      },
      "ManagedPolicyArns": [
        {
          "Fn::Join": [
            "",
            [
              "arn:",
              {
                "Ref": "AWS::Partition"
              },
              ":iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
            ]
          ]
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
    }
  },
  "DeploymentsManualApprovalLambdaRoleDefaultPolicy661B91D6": {
    "Type": "AWS::IAM::Policy",
    "Properties": {
      "PolicyDocument": {
        "Statement": [
          {
            "Action": [
              "s3:GetBucket*",
              "s3:GetObject*",
              "s3:List*"
            ],
            "Effect": "Allow",
            "Resource": [
              {
                "Fn::GetAtt": [
                  "DeploymentsManualApprovalBucket4C81F2B1",
                  "Arn"
                ]
              },
              {
                "Fn::Join": [
                  "",
                  [
                    {
                      "Fn::GetAtt": [
                        "DeploymentsManualApprovalBucket4C81F2B1",
                        "Arn"
                      ]
                    },
                    "/*"
                  ]
                ]
              }
            ]
          },
          {
            "Action": "ecs:ListServiceDeployments",
            "Effect": "Allow",
            "Resource": "*"
          }
        ],
        "Version": "2012-10-17"
      },
      "PolicyName": "DeploymentsManualApprovalLambdaRoleDefaultPolicy661B91D6",
      "Roles": [
        {
          "Ref": "DeploymentsManualApprovalLambdaRole539C07A1"
        }
      ]
    }
  },
  "DeploymentsManualApprovalLambdaAAA9D0C2": {
    "Type": "AWS::Lambda::Function",
    "Properties": {
      "Code": {
        "ZipFile": "import os\nimport boto3\nfrom botocore.exceptions import ClientError\n\ndef lambda_handler(event, context):\n    print(f\"Manual approval event: {event}\")\n    \n    if \"S3_BUCKET\" not in os.environ:\n        print(\"S3_BUCKET environment variable not found\")\n        return {\"hookStatus\": \"FAILED\"}\n    \n    s3_bucket = os.environ.get(\"S3_BUCKET\")\n    \n    # Validate required event fields\n    for var in [\"targetServiceRevisionArn\"]:\n        if var not in event[\"executionDetails\"]:\n            print(f\"Event is missing required {var}\")\n            return {\"hookStatus\": \"FAILED\"}\n    \n    service_revision = event[\"executionDetails\"][\"targetServiceRevisionArn\"]\n    revision_id = service_revision.split(\"/\")[-1]\n        \n    # Check for approval file in S3\n    approval = check_approval_file(s3_bucket, revision_id)\n    if approval != None:\n        print(f\"{revision_id} - Approval file found: {approval}\")\n        return {\"hookStatus\": approval}\n    \n    print(f\"{revision_id} - Approval file not found, waiting for manual approval\")\n    return {\n        \"hookStatus\": \"IN_PROGRESS\",\n        \"callBackDelay\": 30,\n    }\n\ndef check_approval_file(s3_bucket, revision_id):\n    \"\"\"Check if approval file exists in S3\"\"\"\n    # Extract revision ID from ARN\n    file_name = f\"approvals/{revision_id}.txt\"\n    \n    print(f\"{revision_id} - Checking for approval file: s3://{s3_bucket}/{file_name}\")\n    \n    try:\n        s3_client = boto3.client(\"s3\")\n        obj = s3_client.get_object(Bucket=s3_bucket, Key=file_name)\n        if obj[\"Body\"].read().decode(\"utf-8\").strip() != \"approved\":\n            print(f\"{revision_id} - Check failed!: {file_name}\")\n            return \"FAILED\"\n\n        print(f\"{revision_id} - Check succeded!: {file_name}\")\n        return \"SUCCEEDED\"\n        \n    except ClientError as e:\n        print(f\"{revision_id} - Approval file not found yet!: {file_name}\")\n        return None"
      },
      "Environment": {
        "Variables": {
          "S3_BUCKET": {
            "Ref": "DeploymentsManualApprovalBucket4C81F2B1"
          }
        }
      },
      "FunctionName": "ecs-blue-green-manual-approval",
      "Handler": "index.lambda_handler",
      "Role": {
        "Fn::GetAtt": [
          "DeploymentsManualApprovalLambdaRole539C07A1",
          "Arn"
        ]
      },
      "Runtime": "python3.12",
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
      "Timeout": 60
    },
    "DependsOn": [
      "DeploymentsManualApprovalBucketPolicyA1F410F9",
      "DeploymentsManualApprovalBucket4C81F2B1",
      "DeploymentsManualApprovalLambdaRoleDefaultPolicy661B91D6",
      "DeploymentsManualApprovalLambdaRole539C07A1"
    ]
  }
}
```

---
## 1. `"DeploymentsManualApprovalBucket4C81F2B1"`

```json
  "DeploymentsManualApprovalBucket4C81F2B1": {
    "Type": "AWS::S3::Bucket",
    "Properties": {
      "BucketEncryption": {
        "ServerSideEncryptionConfiguration": [
          {
            "ServerSideEncryptionByDefault": {
              "SSEAlgorithm": "AES256"
            }
          }
        ]
      },
      "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "BlockPublicPolicy": true,
        "IgnorePublicAcls": true,
        "RestrictPublicBuckets": true
      },
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
  },
```

S3 버킷을 생성합니다. 이 리소스는 ECS 블루/그린 배포 과정에서 수동 승인 상태를 확인하기 위한 S3 버킷입니다. 배포 중 호출되는 Lambda 함수가 이 버킷 안의 승인 파일을 확인하고, 해당 파일의 내용이 승인 상태이면 배포를 계속 진행합니다. 따라서 이 버킷은 배포 승인 여부를 판단하기 위한 파일을 저장하는 용도로 사용됩니다.

승인 파일은 ECS 블루/그린 배포를 계속 진행해도 되는지 표시하기 위해 S3 버킷에 저장하는 텍스트 파일입니다. 배포 중 호출되는 Lambda 함수는 이 버킷에서 해당 배포 리비전에 대응하는 승인 파일을 찾고, 파일 내용이 `approved`이면 배포를 승인된 것으로 판단합니다.

>이 파일이 존재한다는 사실만으로 AWS가 실제 승인 여부를 자동으로 판단하는 것은 아닙니다. 이 워크숍에서는 사람이 배포를 승인하려면 정해진 위치에 정해진 내용의 파일을 S3에 저장하도록 약속해 둔 것입니다. Lambda 함수는 그 파일이 있는지, 그리고 내용이 `approved`인지 확인한 뒤, 해당 배포가 수동 승인되었다고 간주합니다. 즉, S3 파일은 승인 여부를 직접 판단하는 주체가 아니라 사람이 남겨 둔 승인 표시 역할을 합니다.

---

```json
      "BucketEncryption": {
        "ServerSideEncryptionConfiguration": [
          {
            "ServerSideEncryptionByDefault": {
              "SSEAlgorithm": "AES256"
            }
          }
        ]
      },
```

`"BucketEncryption"`는 버킷에 저장되는 객체를 암호화함을 의미합니다. 

`"ServerSideEncryptionConfiguration"`은 S3 버킷에 저장되는 객체를 서버 측에서 자동으로 암호화하도록 설정하는 항목입니다. `"ServerSideEncryptionByDefault"`는 별도의 암호화 방식을 지정하지 않은 객체에 기본으로 적용할 서버 측 암호화 방식을 의미합니다. 여기서는 `"AES256"` 방식이 사용됩니다.

---

```json
      "PublicAccessBlockConfiguration": {
        "BlockPublicAcls": true,
        "BlockPublicPolicy": true,
        "IgnorePublicAcls": true,
        "RestrictPublicBuckets": true
      },
```

`"PublicAccessBlockConfiguration"`은 S3 버킷이 실수로 공개되지 않도록 막는 설정입니다. S3 버킷은 기본적으로 인터넷에 공개되지 않지만, 설정에 따라 인터넷에 공개될 수 있습니다. 예를 들어 버킷 정책이나 ACL을 통해 특정 파일을 외부에서 다운로드할 수 있도록 허용하거나, S3의 정적 웹 사이트 호스팅 기능을 사용하기 위해 버킷을 공개하는 경우가 있습니다. 이 설정은 그런 공개 접근이 의도치 않게 허용되는 것을 방지합니다.

참고로 ACL(Access Control List)은 S3 버킷이나 객체에 적용할 수 있는 접근 제어 목록입니다. 어떤 사용자나 계정이 해당 버킷 또는 객체를 읽거나 쓸 수 있는지를 지정하는 방식입니다. 예를 들어 `public-read` ACL을 설정하면 객체가 외부에 공개될 수 있습니다. 다만 ACL은 비교적 오래된 접근 제어 방식이므로, 현재는 IAM 정책이나 버킷 정책을 통해 접근 권한을 관리하는 방식이 더 일반적으로 사용됩니다.

---

`"BlockPublicAcls": true`는 새로운 공개 ACL을 설정하지 못하게 막습니다.

`"BlockPublicPolicy": true`는 S3 버킷을 공개하는 버킷 정책을 새로 설정하지 못하게 막습니다.

`"IgnorePublicAcls": true`는 버킷이나 객체에 공개 ACL이 존재하더라도 해당 공개 권한은 무시됩니다. 즉, 기존 ACL 때문에 버킷이나 객체가 외부에 공개되는 것을 방지합니다.

`"RestrictPublicBuckets": true`는 버킷 정책이 공개 접근을 허용하더라도, 외부 사용자의 접근을 제한하는 설정이야.

위 4개의 설정은 S3 버킷의 공개 접근을 차단하기 위해 함께 설정하는 경우가 많습니다. 네 설정은 각각 ACL과 버킷 정책을 통한 공개 접근을 서로 다른 지점에서 차단합니다. 따라서 이 네 값을 모두 `true`로 설정하면, 버킷이나 객체가 실수로 외부에 공개되는 것을 강하게 방지할 수 있습니다.

---
## 2. `"DeploymentsManualApprovalBucketPolicyA1F410F9"`

```json
  "DeploymentsManualApprovalBucketPolicyA1F410F9": {
    "Type": "AWS::S3::BucketPolicy",
    "Properties": {
      "Bucket": {
        "Ref": "DeploymentsManualApprovalBucket4C81F2B1"
      },
      "PolicyDocument": {
        "Statement": [
          {
            "Action": "s3:*",
            "Condition": {
              "Bool": {
                "aws:SecureTransport": "false"
              }
            },
            "Effect": "Deny",
            "Principal": {
              "AWS": "*"
            },
            "Resource": [
              {
                "Fn::GetAtt": [
                  "DeploymentsManualApprovalBucket4C81F2B1",
                  "Arn"
                ]
              },
              {
                "Fn::Join": [
                  "",
                  [
                    {
                      "Fn::GetAtt": [
                        "DeploymentsManualApprovalBucket4C81F2B1",
                        "Arn"
                      ]
                    },
                    "/*"
                  ]
                ]
              }
            ]
          }
        ],
        "Version": "2012-10-17"
      }
    }
  },
```

버킷 정책을 생성합니다.

---

```json
      "Bucket": {
        "Ref": "DeploymentsManualApprovalBucket4C81F2B1"
      },
```

이전에 생성한 버킷에 이 폴리시를 연결합니다.

---

```json
      "PolicyDocument": {
        "Statement": [
          {
            "Action": "s3:*",
            "Condition": {
              "Bool": {
                "aws:SecureTransport": "false"
              }
            },
            "Effect": "Deny",
            "Principal": {
              "AWS": "*"
            },
```

여기서 `"PolicyDocument"`는 S3 버킷에 붙는 Bucket Policy를 의미합니다.

`"Action": "s3:*"`은 S3에 대해 수행할 수 있는 모든 작업을 의미합니다. 예를 들어 객체를 조회하는 `s3:GetObject`, 객체를 업로드하는 `s3:PutObject`, 버킷의 객체 목록을 조회하는 `s3:ListBucket` 같은 작업이 여기에 포함됩니다.

`"aws:SecureTransport": "false"`는 요청이 안전한 전송 방식을 사용하지 않았을 때를 의미합니다. 여기서 안전한 전송 방식은 일반적으로 HTTPS를 의미합니다. 따라서 이 조건에 해당하는 요청, 즉 HTTPS가 아닌 방식으로 들어온 S3 요청은 `"Effect": "Deny"`에 의해 거부됩니다. `"Action": "s3:*"`로 설정되어 있으므로, 이 조건에 해당하면 모든 S3 작업이 거부됩니다.

`"Principal"`은 `"AWS": "*"`로 설정되어 있으므로, 특정 사용자나 Role이 아니라 모든 AWS 주체에게 이 정책이 적용됩니다. 이 버킷 정책의 목적은 요청자가 누구인지와 상관없이 HTTPS가 아닌 S3 요청을 거부하는 것이므로, `Principal`을 전체 대상으로 지정합니다. 

따라서 이 정책은 어떤 요청자(IAM 사용자, IAM Role, AWS 서비스, 애플리케이션 등)라도 HTTPS가 아닌 안전하지 않은 방식으로 이 S3 버킷이나 버킷 안의 객체에 접근하려 하면 해당 요청을 거부합니다.

---

```json
            "Resource": [
              {
                "Fn::GetAtt": [
                  "DeploymentsManualApprovalBucket4C81F2B1",
                  "Arn"
                ]
              },
              {
                "Fn::Join": [
                  "",
                  [
                    {
                      "Fn::GetAtt": [
                        "DeploymentsManualApprovalBucket4C81F2B1",
                        "Arn"
                      ]
                    },
                    "/*"
                  ]
                ]
              }
            ]
```

첫 번째 Resource는 버킷 자체를 의미하고, 두 번째 Resource는 버킷 안의 모든 객체를 의미합니다. 따라서 이 정책은 버킷 자체와 버킷 안의 객체 모두에 적용됩니다.

S3에서는 버킷 자체와 버킷 안의 객체가 서로 다른 리소스 ARN으로 표현됩니다. 버킷 자체는 `arn:aws:s3:::버킷이름` 형식이고, 버킷 안의 객체는 `arn:aws:s3:::버킷이름/*` 형식입니다. 이 정책은 `"Action": "s3:*"`로 모든 S3 작업을 대상으로 하므로, 버킷 수준 작업과 객체 수준 작업을 모두 포함합니다. 따라서 `"Resource"`에는 버킷 자체 ARN과 버킷 안의 모든 객체 ARN을 함께 지정합니다.

---
## 3. `"DeploymentsECSInfrastructureRole1FA52583"`

```json
  "DeploymentsECSInfrastructureRole1FA52583": {
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
      "Description": "ECS Infrastructure role for managing load balancer resources during blue-green deployments",
      "ManagedPolicyArns": [
        {
          "Fn::Join": [
            "",
            [
              "arn:",
              {
                "Ref": "AWS::Partition"
              },
              ":iam::aws:policy/AmazonECSInfrastructureRolePolicyForLoadBalancers"
            ]
          ]
        }
      ],
      "RoleName": "ecsInfrastructureRoleForLoadBalancers",
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

IAM Role을 생성합니다. 이 IAM Role은 ECS가 블루/그린 배포 중 로드 밸런서 관련 리소스를 관리하기 위해 사용하는 IAM Role입니다. 

---

`"AmazonECSInfrastructureRolePolicyForLoadBalancers"`는 ECS가 사용자를 대신해 Elastic Load Balancing 리소스를 관리할 수 있도록 제공되는 AWS 관리형 정책입니다. AWS 문서에 따르면 이 정책에는 Listener, Rule, Target Group, Target Health 조회 권한, Target Group에 Target을 등록하거나 해제하는 권한, ALB/NLB Listener 수정 권한, ALB Rule 수정 권한이 포함됩니다.

다음은 위 IAM Role에 연결된 AWS 관리형 정책의 세부 내역의 일부 입니다.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ELBReadOperations",
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:DescribeListeners",
                "elasticloadbalancing:DescribeRules",
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:DescribeTargetHealth"
            ],
            "Resource": "*"
        },
		...
}
```

`"Sid"`는 Statement ID를 의미합니다. 

`"elasticloadbalancing:DescribeListeners"`는 로드 밸런서의 Listener 정보를 조회하는 권한입니다.

`"elasticloadbalancing:DescribeRules"`는 로드 밸런서 Listener에 연결된 Rule 정보를 조회하는 권한입니다. 

`"elasticloadbalancing:DescribeTargetGroups"`는 Target Group 정보를 조회하는 권한입니다. Target Group은 로드 밸런서가 요청을 전달할 대상들의 묶음입니다. 

`"elasticloadbalancing:DescribeTargetHealth"`는 Target Group에 등록된 대상들의 상태를 조회하는 권한입니다. 

주의할 점으로 Target Group은 Load Balancer 내부에 포함된 하위 구성 요소라기보다는, Load Balancer와 별도로 존재하는 리소스입니다. 반면 Listener와 Rule은 Load Balancer에 연결되어 동작하는 구성 요소입니다. Listener는 요청을 받을 포트와 프로토콜을 정의하고, Rule은 요청을 어떤 조건에 따라 어느 Target Group으로 전달할지 정의합니다. 따라서 Load Balancer는 Listener와 Rule을 통해 별도로 존재하는 Target Group으로 트래픽을 전달합니다.

---
## 4. `"DeploymentsECSLifecycleHookRole2D97E826"`

```json
  "DeploymentsECSLifecycleHookRole2D97E826": {
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
      "Description": "Role for ECS to invoke Lambda functions during lifecycle hooks",
      "RoleName": "ECSLifecycleHookRole",
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

IAM Role을 생성합니다. 이 IAM Role은 ECS가 Lifecycle Hook 중 Lambda 함수를 호출하기 위해 사용하는 IAM Role입니다. 배포 과정에서 ECS가 특정 시점에 Lambda 함수를 호출해야 할 때 이 Role을 사용합니다.

Lifecycle Hook이란 배포 과정의 특정 시점에 추가 작업을 실행할 수 있도록 마련된 연결 지점입니다. ECS 배포에서는 Lifecycle Hook을 통해 배포 중 Lambda 함수를 호출하고, 새 버전 검증이나 수동 승인 확인 같은 작업을 수행할 수 있습니다. 따라서 Lifecycle Hook은 배포 흐름 중간에 검증이나 승인 단계를 끼워 넣기 위한 장치라고 볼 수 있습니다.

Lifecycle Hook은 실무에서도 배포 과정의 검증이나 추가 작업을 위해 사용될 수 있습니다. 다만 이 템플릿처럼 S3 버킷에 특정 텍스트 파일을 저장해 수동 승인 여부를 표시하는 방식은 실무 표준이라기보다는 워크숍용 예제에 가까운 구현입니다. 실무에서는 보통 CodePipeline, CodeDeploy, GitHub Actions 같은 배포 도구의 승인 단계나 CloudWatch Alarm, 헬스 체크, 자동 테스트 결과를 이용해 배포 진행 여부를 결정합니다.

---
## 5. `"DeploymentsECSLifecycleHookRoleDefaultPolicyFB30EB40"`

```json
  "DeploymentsECSLifecycleHookRoleDefaultPolicyFB30EB40": {
    "Type": "AWS::IAM::Policy",
    "Properties": {
      "PolicyDocument": {
        "Statement": [
          {
            "Action": "lambda:InvokeFunction",
            "Effect": "Allow",
            "Resource": {
              "Fn::GetAtt": [
                "DeploymentsManualApprovalLambdaAAA9D0C2",
                "Arn"
              ]
            }
          }
        ],
        "Version": "2012-10-17"
      },
      "PolicyName": "DeploymentsECSLifecycleHookRoleDefaultPolicyFB30EB40",
      "Roles": [
        {
          "Ref": "DeploymentsECSLifecycleHookRole2D97E826"
        }
      ]
    }
  },
```

IAM Policy를 생성합니다. 는 ECS Lifecycle Hook Role에 Lambda 호출 권한을 부여합니다. ECS는 Lifecycle Hook 단계에서 수동 승인 확인용 Lambda 함수를 호출할 수 있습니다.

---

`"Action": "lambda:InvokeFunction"`는 이 정책이 Lambda 함수를 호출할 수 있음을 의미합니다.

---

대상 리소스는 `"DeploymentsManualApprovalLambdaAAA9D0C2"`입니다.

---
## 6. `"DeploymentsManualApprovalLambdaRole539C07A1"`

```json
  "DeploymentsManualApprovalLambdaRole539C07A1": {
    "Type": "AWS::IAM::Role",
    "Properties": {
      "AssumeRolePolicyDocument": {
        "Statement": [
          {
            "Action": "sts:AssumeRole",
            "Effect": "Allow",
            "Principal": {
              "Service": "lambda.amazonaws.com"
            }
          }
        ],
        "Version": "2012-10-17"
      },
      "ManagedPolicyArns": [
        {
          "Fn::Join": [
            "",
            [
              "arn:",
              {
                "Ref": "AWS::Partition"
              },
              ":iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
            ]
          ]
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
    }
  },
```

IAM Role을 생성합니다. 이 IAM Role은 수동 승인 확인용 Lambda 함수가 실행될 때 사용합니다.

---

`"AWSLambdaBasicExecutionRole"` 정책은 Lambda가 CloudWatch Logs에 실행 로그를 남길 수 있습니다. 

---
## 7. `"DeploymentsManualApprovalLambdaRoleDefaultPolicy661B91D6"`

```json
  "DeploymentsManualApprovalLambdaRoleDefaultPolicy661B91D6": {
    "Type": "AWS::IAM::Policy",
    "Properties": {
      "PolicyDocument": {
        "Statement": [
          {
            "Action": [
              "s3:GetBucket*",
              "s3:GetObject*",
              "s3:List*"
            ],
            "Effect": "Allow",
            "Resource": [
              {
                "Fn::GetAtt": [
                  "DeploymentsManualApprovalBucket4C81F2B1",
                  "Arn"
                ]
              },
              {
                "Fn::Join": [
                  "",
                  [
                    {
                      "Fn::GetAtt": [
                        "DeploymentsManualApprovalBucket4C81F2B1",
                        "Arn"
                      ]
                    },
                    "/*"
                  ]
                ]
              }
            ]
          },
          {
            "Action": "ecs:ListServiceDeployments",
            "Effect": "Allow",
            "Resource": "*"
          }
        ],
        "Version": "2012-10-17"
      },
      "PolicyName": "DeploymentsManualApprovalLambdaRoleDefaultPolicy661B91D6",
      "Roles": [
        {
          "Ref": "DeploymentsManualApprovalLambdaRole539C07A1"
        }
      ]
    }
  },
```

IAM Policy를 생성합니다.

---

```json
        "Statement": [
          {
            "Action": [
              "s3:GetBucket*",
              "s3:GetObject*",
              "s3:List*"
            ],
            "Effect": "Allow",
            "Resource": [
              {
                "Fn::GetAtt": [
                  "DeploymentsManualApprovalBucket4C81F2B1",
                  "Arn"
                ]
              },
              {
                "Fn::Join": [
                  "",
                  [
                    {
                      "Fn::GetAtt": [
                        "DeploymentsManualApprovalBucket4C81F2B1",
                        "Arn"
                      ]
                    },
                    "/*"
                  ]
                ]
              }
            ]
          },
```

`"Action"`에는 Lambda 함수가 수동 승인용 S3 버킷을 조회하기 위해 필요한 S3 권한이 지정되어 있습니다. `"s3:GetBucket*"`은 버킷의 설정이나 속성 정보를 조회하는 권한이고, `"s3:GetObject*"`은 버킷 안의 객체 내용을 조회하는 권한입니다. `"s3:List*"`은 버킷 안의 객체 목록을 조회하는 권한입니다. 이 권한들을 통해 Lambda는 승인 여부를 나타내는 파일이 존재하는지 확인하고, 해당 파일의 내용을 읽을 수 있습니다.

---

```json
            "Resource": [
              {
                "Fn::GetAtt": [
                  "DeploymentsManualApprovalBucket4C81F2B1",
                  "Arn"
                ]
              },
              {
                "Fn::Join": [
                  "",
                  [
                    {
                      "Fn::GetAtt": [
                        "DeploymentsManualApprovalBucket4C81F2B1",
                        "Arn"
                      ]
                    },
                    "/*"
                  ]
                ]
              }
            ]
```

`"Resource"`에는 Lambda 함수가 조회할 수 있는 S3 리소스가 지정됩니다. 첫 번째 값은 `"DeploymentsManualApprovalBucket4C81F2B1"` 버킷 자체의 ARN이고, 두 번째 값은 그 ARN 뒤에 `"/*"`를 붙여 버킷 안의 모든 객체를 대상으로 지정합니다. 따라서 Lambda는 수동 승인용 S3 버킷 자체와 그 안의 객체를 조회할 수 있습니다.

---

```json
          {
            "Action": "ecs:ListServiceDeployments",
            "Effect": "Allow",
            "Resource": "*"
          }
```

`"ecs:ListServiceDeployments"`는 ECS 서비스의 배포 목록을 조회하는 권한입니다. 이 Lambda 함수는 수동 승인 여부를 판단하는 과정에서 현재 ECS 서비스 배포 정보를 확인해야 하므로 이 권한을 사용합니다. `"Effect"`가 `"Allow"`로 설정되어 있으므로 해당 작업이 허용되며, `"Resource"`가 `"*"`로 설정되어 있어 특정 리소스 하나로 제한하지 않고 조회할 수 있습니다.

---
## 8. `"DeploymentsManualApprovalLambdaAAA9D0C2"`

```json
  "DeploymentsManualApprovalLambdaAAA9D0C2": {
    "Type": "AWS::Lambda::Function",
    "Properties": {
      "Code": {
        "ZipFile": "import os\nimport boto3\nfrom botocore.exceptions import ClientError\n\ndef lambda_handler(event, context):\n    print(f\"Manual approval event: {event}\")\n    \n    if \"S3_BUCKET\" not in os.environ:\n        print(\"S3_BUCKET environment variable not found\")\n        return {\"hookStatus\": \"FAILED\"}\n    \n    s3_bucket = os.environ.get(\"S3_BUCKET\")\n    \n    # Validate required event fields\n    for var in [\"targetServiceRevisionArn\"]:\n        if var not in event[\"executionDetails\"]:\n            print(f\"Event is missing required {var}\")\n            return {\"hookStatus\": \"FAILED\"}\n    \n    service_revision = event[\"executionDetails\"][\"targetServiceRevisionArn\"]\n    revision_id = service_revision.split(\"/\")[-1]\n        \n    # Check for approval file in S3\n    approval = check_approval_file(s3_bucket, revision_id)\n    if approval != None:\n        print(f\"{revision_id} - Approval file found: {approval}\")\n        return {\"hookStatus\": approval}\n    \n    print(f\"{revision_id} - Approval file not found, waiting for manual approval\")\n    return {\n        \"hookStatus\": \"IN_PROGRESS\",\n        \"callBackDelay\": 30,\n    }\n\ndef check_approval_file(s3_bucket, revision_id):\n    \"\"\"Check if approval file exists in S3\"\"\"\n    # Extract revision ID from ARN\n    file_name = f\"approvals/{revision_id}.txt\"\n    \n    print(f\"{revision_id} - Checking for approval file: s3://{s3_bucket}/{file_name}\")\n    \n    try:\n        s3_client = boto3.client(\"s3\")\n        obj = s3_client.get_object(Bucket=s3_bucket, Key=file_name)\n        if obj[\"Body\"].read().decode(\"utf-8\").strip() != \"approved\":\n            print(f\"{revision_id} - Check failed!: {file_name}\")\n            return \"FAILED\"\n\n        print(f\"{revision_id} - Check succeded!: {file_name}\")\n        return \"SUCCEEDED\"\n        \n    except ClientError as e:\n        print(f\"{revision_id} - Approval file not found yet!: {file_name}\")\n        return None"
      },
      "Environment": {
        "Variables": {
          "S3_BUCKET": {
            "Ref": "DeploymentsManualApprovalBucket4C81F2B1"
          }
        }
      },
      "FunctionName": "ecs-blue-green-manual-approval",
      "Handler": "index.lambda_handler",
      "Role": {
        "Fn::GetAtt": [
          "DeploymentsManualApprovalLambdaRole539C07A1",
          "Arn"
        ]
      },
      "Runtime": "python3.12",
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
      "Timeout": 60
    },
    "DependsOn": [
      "DeploymentsManualApprovalBucketPolicyA1F410F9",
      "DeploymentsManualApprovalBucket4C81F2B1",
      "DeploymentsManualApprovalLambdaRoleDefaultPolicy661B91D6",
      "DeploymentsManualApprovalLambdaRole539C07A1"
    ]
  }
```

Lambda 함수를 생성합니다. 이 Lambda 함수는 ECS 블루/그린 배포 중 수동 승인 여부를 확인합니다.

---

```json
      "Environment": {
        "Variables": {
          "S3_BUCKET": {
            "Ref": "DeploymentsManualApprovalBucket4C81F2B1"
          }
        }
      },
```

`"Environment"`는 환경 변수를 의미합니다.

`"S3_BUCKET"`은 환경 변수의 이름을 의미합니다. 이 환경 변수에 `"DeploymentsManualApprovalBucket4C81F2B1"` 버킷 이름을 넣습니다.

이로 인해 Lambda 함수는 실행 중 이 환경 변수를 읽어 수동 승인 여부를 확인할 S3 버킷을 찾을 수 있습니다.

---

```json
      "FunctionName": "ecs-blue-green-manual-approval",
      "Handler": "index.lambda_handler",
      "Role": {
        "Fn::GetAtt": [
          "DeploymentsManualApprovalLambdaRole539C07A1",
          "Arn"
        ]
      },
      "Runtime": "python3.12",
```

`"FunctionName"`은 이 Lambda 함수의 이름을 의미합니다.

`"Handler"`는 Lambda 함수가 호출될 때 실행을 시작할 함수의 위치를 지정하는 설정입니다. 여기서는 `"index"` 파일 안의 `"lambda_handler"` 함수를 실행합니다.

Lambda 함수는 실행 중에 AWS 리소스에 접근해야 할 수 있기 때문에, IAM Role을 지정해 주어야 합니다. 여기서는 `"DeploymentsManualApprovalLambdaRole539C07A1"` IAM Role을 지정합니다.

`"Runtime": "python3.12"`는 이 Lambda 함수가 Python 3.12 런타임 환경에서 실행된다는 의미입니다. 즉, Lambda 서비스는 이 함수 코드를 Python 3.12 기준으로 실행합니다.

---

