---
title: "04. 암호화 및 시크릿 (KMS & Secrets Manager)"
weight: 4
---
> **작성일:** 2026-05-05 | **수정일:** 2026-05-05
```json
{
  "NetworkKMSKey37A7DAB2": {
   "Type": "AWS::KMS::Key",
   "Properties": {
    "Description": "retail store ecs CMK",
    "EnableKeyRotation": true,
    "KeyPolicy": {
     "Statement": [
      {
       "Action": "kms:*",
       "Effect": "Allow",
       "Principal": {
        "AWS": {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":iam::",
           {
            "Ref": "AWS::AccountId"
           },
           ":root"
          ]
         ]
        }
       },
       "Resource": "*"
      },
      {
       "Action": [
        "kms:CreateGrant",
        "kms:Decrypt",
        "kms:DescribeKey",
        "kms:Encrypt",
        "kms:GenerateDataKey*",
        "kms:ReEncrypt*"
       ],
       "Condition": {
        "StringEquals": {
         "kms:ViaService": {
          "Fn::Join": [
           "",
           [
            "secretsmanager.",
            {
             "Ref": "AWS::Region"
            },
            ".amazonaws.com"
           ]
          ]
         }
        }
       },
       "Effect": "Allow",
       "Principal": {
        "AWS": {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":iam::",
           {
            "Ref": "AWS::AccountId"
           },
           ":root"
          ]
         ]
        }
       },
       "Resource": "*"
      }
     ],
     "Version": "2012-10-17"
    },
    "PendingWindowInDays": 7,
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
  "NetworkKMSKeyAliasB2B35753": {
   "Type": "AWS::KMS::Alias",
   "Properties": {
    "AliasName": "alias/retail-store-ecs-cmk",
    "TargetKeyId": {
     "Fn::GetAtt": [
      "NetworkKMSKey37A7DAB2",
      "Arn"
     ]
    }
   }
  },
  "NetworkDBCredentialsSecretADE5ABB3": {
   "Type": "AWS::SecretsManager::Secret",
   "Properties": {
    "GenerateSecretString": {
     "ExcludePunctuation": true,
     "GenerateStringKey": "password",
     "IncludeSpace": false,
     "PasswordLength": 10,
     "SecretStringTemplate": "{\"username\":\"postgres\"}"
    },
    "KmsKeyId": {
     "Fn::GetAtt": [
      "NetworkKMSKey37A7DAB2",
      "Arn"
     ]
    },
    "Name": "retail-store-ecs-orders-db",
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
  "NetworkDBCredentialsSecretAttachment41F10924": {
   "Type": "AWS::SecretsManager::SecretTargetAttachment",
   "Properties": {
    "SecretId": {
     "Ref": "NetworkDBCredentialsSecretADE5ABB3"
    },
    "TargetId": {
     "Ref": "NetworkOrdersCluster095F1433"
    },
    "TargetType": "AWS::RDS::DBCluster"
   }
  },
  "NetworkCatalogDBCredentialsSecret3F8AA865": {
   "Type": "AWS::SecretsManager::Secret",
   "Properties": {
    "GenerateSecretString": {
     "ExcludePunctuation": true,
     "GenerateStringKey": "password",
     "IncludeSpace": false,
     "PasswordLength": 10,
     "SecretStringTemplate": "{\"username\":\"root\"}"
    },
    "KmsKeyId": {
     "Fn::GetAtt": [
      "NetworkKMSKey37A7DAB2",
      "Arn"
     ]
    },
    "Name": "retail-store-ecs-catalog-db",
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
  "NetworkCatalogDBCredentialsSecretAttachment39F70EC9": {
   "Type": "AWS::SecretsManager::SecretTargetAttachment",
   "Properties": {
    "SecretId": {
     "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
    },
    "TargetId": {
     "Ref": "NetworkCatalogClusterB0717C37"
    },
    "TargetType": "AWS::RDS::DBCluster"
   }
  }
}
```

---

## 1. `"NetworkKMSKey37A7DAB2"`

```json
  "NetworkKMSKey37A7DAB2": {
   "Type": "AWS::KMS::Key",
   "Properties": {
    "Description": "retail store ecs CMK",
    "EnableKeyRotation": true,
    "KeyPolicy": {
     "Statement": [
      {
       "Action": "kms:*",
       "Effect": "Allow",
       "Principal": {
        "AWS": {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":iam::",
           {
            "Ref": "AWS::AccountId"
           },
           ":root"
          ]
         ]
        }
       },
       "Resource": "*"
      },
      {
       "Action": [
        "kms:CreateGrant",
        "kms:Decrypt",
        "kms:DescribeKey",
        "kms:Encrypt",
        "kms:GenerateDataKey*",
        "kms:ReEncrypt*"
       ],
       "Condition": {
        "StringEquals": {
         "kms:ViaService": {
          "Fn::Join": [
           "",
           [
            "secretsmanager.",
            {
             "Ref": "AWS::Region"
            },
            ".amazonaws.com"
           ]
          ]
         }
        }
       },
       "Effect": "Allow",
       "Principal": {
        "AWS": {
         "Fn::Join": [
          "",
          [
           "arn:",
           {
            "Ref": "AWS::Partition"
           },
           ":iam::",
           {
            "Ref": "AWS::AccountId"
           },
           ":root"
          ]
         ]
        }
       },
       "Resource": "*"
      }
     ],
     "Version": "2012-10-17"
    },
    "PendingWindowInDays": 7,
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

KMS Key를 생성하는 리소스입니다. AWS KMS는 Key Management Service의 약자로, 기밀 데이터를 암호화 및 복호화하는 키를 관리하는 서비스입니다.

---

`"Description": "retail store ecs CMK"`을 보면 이 KMS 키가 우리가 사용하는 Retail Store Sample App의 ECS를 위한 CMK라는 것을 알 수 있습니다. 여기서 CMK는 예전 AWS KMS 용어인 Customer Master Key를 의미하며, 현재는 KMS key라고 부릅니다. 이 템플릿처럼 사용자가 직접 생성하고 관리하는 KMS 키는 Customer managed key라고 부릅니다. 주의할 점으로, 여기서 말하는 KMS 키는 데이터 키가 아니라, 예전 용어로 마스터키라고 불리던 KMS key를 가리킵니다. 

참고로 AWS가 생성하고 관리하는 KMS 키는 AWS managed key라고 불립니다. Secrets Manager, S3, EBS 같은 서비스에서 기본 암호화 기능을 켜면 AWS managed key가 사용될 수 있습니다. AWS owned key라는 것도 존재하는데, 마찬가지로 AWS가 생성하고 관리하는 KMS 키이지만 사용자가 직접 접근하거나 관리할 수 없습니다.

---

`"EnableKeyRotation": true`는 새 버전의 KMS 키를 주기적으로 생성한다는 의미입니다. 기존의 키는 기존에 해당 키로 암호화된 데이터를 복호화하기 위해 보관됩니다. 같은 키 버전을 계속 사용하지 않기 때문에 키 노출로 인한 위험을 줄일 수 있습니다.

---

`"KeyPolicy"`는 이 KMS 키에 적용될 정책을 의미합니다.

`"Statement"`이 정책에 적용될 규칙을 의미합니다.

---

이 블록은 `"Statement"`의 첫 번째 내용에 대한 설명입니다.

`"Action": "kms:*"`는 KMS 서비스에서 수행할 수 있는 모든 작업을 의미합니다.

`"Principal"`의 `"AWS"` 필드는 AWS 계정을 포함한 AWS IAM Principal을 지정하는 필드입니다. AWS IAM Principal은 AWS에서 권한을 받을 수 있는 대상을 의미합니다. IAM User, IAM Role, AWS Account 등이 이 대상이 될 수 있습니다.

`"AWS"` 필드의 값에서, `AWS::AccountId`는 이 CloudFormation 스택이 배포되는 AWS 계정의 Account ID를 의미합니다. `":root"`는 루트 권한이 아니라, 해당 AWS 계정 자체를 대표하는 root principal을 의미합니다. 즉, 이 `"Principal"` 필드의 값은 이 CloudFormation 스택이 배포되는 AWS 계정을 가리킵니다. 만약 `":root"` 대신에 `":user/MyUser"` 같은 값이 오면, 이는 해당 AWS 계정 안의 특정 사용자를 가리킵니다.

참고로 "배포하는 계정"이 아니라 "배포되는 계정"이라는 표현을 사용한 것은, 다른 사용자가 다른 AWS 계정의 권한을 위임받아서 배포한다면, 배포하는 계정에 스택이 생성되는 것이 아니기 때문입니다.

`"Resource": "*"`는 이 `"Action"`이 취해질 수 있는 대상이 제한되어 있지 않다는 것을 의미합니다.

요약하자면, 이 CloudFormation 스택이 배포되는 AWS 계정은 이 KMS Key에 대해 KMS 서비스에서 수행할 수 있는 모든 작업을 수행할 수 있고, 그 작업의 대상이 되는 리소스 또한 제한되어 있지 않습니다.

---

이 블록은 `"Statement"`의 두 번째 내용에 대한 설명입니다.

`"Action"`을 보면 `"Statement"`의 첫 번째 내용과 다르게 KMS 서비스에서 수행할 수 있는 모든 작업이 아니라 일부 작업만 명시합니다.

`"kms:CreateGrant"`는 이 KMS 키에 대한 사용 권한을 다른 대상(AWS 서비스나 AWS IAM Principal)에게 위임하는 것을 의미합니다. AWS Secrets Manager 같은 서비스가 이 KMS 키를 대신 사용할 수 있게 하기 위해서 입니다.

`"kms:Decrypt"`는 데이터 키를 복호화하는 작업을 의미합니다.

`"kms:DescribeKey"`는 KMS 키의 메타데이터를 조회하는 작업을 의미합니다.

`"kms:Encrypt"`는 데이터 키를 암호화하는 작업을 의미합니다.

`"kms:GenerateDataKey*"`는 데이터 키를 생성하는 작업과 관련된 모든 작업을 의미합니다.

`"kms:ReEncrypt*"`는 기존 KMS 키로 암호화된 데이터 키를 복호화한 뒤, 다른 KMS 키로 다시 암호화하는 작업과 관련된 모든 작업을 의미합니다.

`"Condition"`은 지정된 조건이 만족될 때만 해당 `"Statement"`의 내용이 적용됨을 의미합니다.

`"StringEquals"`는 지정된 문자열과 같을 때를 의미합니다.

`"kms:ViaService"`는 KMS 요청이 지정된 AWS 서비스를 통해 들어왔을 때를 의미합니다.

`"Fn::Join"`은 현재 리전의 Secrets Manager 서비스 주소를 만듭니다.

이 "Condition"을 요약하자면, KMS 요청이 현재 리전의 Secrets Manager로부터 왔을 때를 의미합니다.

`"Principal"`은 `"Statement"`의 첫 번째 내용과 마찬가지로 이 CloudFormation 스택이 배포되는 AWS 계정을 가리킵니다

`"Resource": "*"` 이기 때문에, `"Action"`이 취해질 수 있는 대상이 제한되어 있지 않습니다.

요약하자면, 이 CloudFormation 스택이 배포되는 AWS 계정은 AWS Secrets Manager를 통해 KMS 요청이 들어왔을 때, Secrets Manager가 특정 작업들(`"kms:CreateGrant"` 등)에 한해 이 KMS 키를 사용할 수 있게 허락합니다. 이때 KMS 작업의 대상이 되는 리소스는 제한되지 않습니다.

`"Statement"`의 첫 번째 내용은 AWS 계정이 KMS 키를 사용하게 해주는 것이고, 두 번째 내용은 AWS Secrets Manager를 통해 KMS 요청이 들어왔을 때, 이 AWS 계정이 KMS의 특정 작업을 수행할 수 있게 허용하는 것입니다.

---

`"Version"`은 IAM Policy의 문법 버전을 의미합니다. 이 리소스는 IAM Policy는 아니지만, IAM Policy가 사용하는 문법과 동일한 문법을 사용합니다. 여기서는 `"2012-10-17"` 버전의 정책 문법을 사용합니다.

---

`"PendingWindowInDays": 7`은 KMS 키를 삭제 예약했을 때, 실제 삭제 시까지 보관하는 기간이 `7`일 이라는 의미입니다.

---
## 2. `"NetworkKMSKeyAliasB2B35753"`

```json
  "NetworkKMSKeyAliasB2B35753": {
   "Type": "AWS::KMS::Alias",
   "Properties": {
    "AliasName": "alias/retail-store-ecs-cmk",
    "TargetKeyId": {
     "Fn::GetAtt": [
      "NetworkKMSKey37A7DAB2",
      "Arn"
     ]
    }
   }
  },
```

KMS 키의 Alias를 생성하는 리소스입니다. KMS 키의 Key ID는 의미 없는 복잡한 문자열이기 때문에, Alias를 통해 쉽게 접근할 수 있습니다.

---

`"AliasName": "alias/retail-store-ecs-cmk"`는 이 Alias의 이름으로 `retail-store-ecs-cmk`를 지정합니다.

---

`"TargetKeyId"`는 이 Alias가 가리킬 KMS 키를 지정합니다.

---

## 3. `"NetworkDBCredentialsSecretADE5ABB3"`

```json
  "NetworkDBCredentialsSecretADE5ABB3": {
   "Type": "AWS::SecretsManager::Secret",
   "Properties": {
    "GenerateSecretString": {
     "ExcludePunctuation": true,
     "GenerateStringKey": "password",
     "IncludeSpace": false,
     "PasswordLength": 10,
     "SecretStringTemplate": "{\"username\":\"postgres\"}"
    },
    "KmsKeyId": {
     "Fn::GetAtt": [
      "NetworkKMSKey37A7DAB2",
      "Arn"
     ]
    },
    "Name": "retail-store-ecs-orders-db",
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

AWS Secrets Manager에 저장되는 Secret을 생성하는 리소스입니다. 이 시크릿은 DB Credentials인 사용자 이름과 사용자 비밀번호를 가지고 있는 DB Secret입니다.

---

`"GenerateSecretString"`은 시크릿 문자열을 자동으로 생성합니다. 설정에 따라 시크릿 문자열의 일부 값을 자동으로 랜덤하게 생성합니다.

`"ExcludePunctuation": true`는 시크릿 문자열 중 랜덤하게 생성되는 문자열에 `!`, `?`, `#` 같은 특수 기호를 쓰지 않겠다는 것을 의미합니다.

`"GenerateStringKey": "password"`는 시크릿 문자열 중 `"password"` 키의 값을 랜덤하게 생성하겠다는 의미입니다. 시크릿 문자열이 `{"username":"postgres","password":"<랜덤 생성된 문자열>"}` 이런 구조를 가지고 있기 때문에 `"password"`는 키가 됩니다.

`"IncludeSpace": false`는 시크릿 문자열 중 랜덤하게 생성되는 문자열에 공백을 포함하지 않겠다는 것을 의미합니다.

`"PasswordLength": 10`은 랜덤하게 생성되는 Password 문자열의 길이를 `10`자로 지정한다는 것을 의미합니다.

`"SecretStringTemplate": "{\"username\":\"postgres\"}"`는 자동으로 생성되는 시크릿 문자열의 템플릿입니다. 여기에 `"password":"<랜덤 생성된 문자열>"`이 템플릿 안에 추가됩니다.

---

`"KmsKeyId"`는 이 시크릿을 암호화하는 데 쓰이는 데이터 키를 보호할(암호화/복호화할) KMS 키를 지정합니다. 앞서 만든 KMS 키를 지정합니다. 

---

## 4. `"NetworkDBCredentialsSecretAttachment41F10924"`

```json
  "NetworkDBCredentialsSecretAttachment41F10924": {
   "Type": "AWS::SecretsManager::SecretTargetAttachment",
   "Properties": {
    "SecretId": {
     "Ref": "NetworkDBCredentialsSecretADE5ABB3"
    },
    "TargetId": {
     "Ref": "NetworkOrdersCluster095F1433"
    },
    "TargetType": "AWS::RDS::DBCluster"
   }
  },
```

Secret Attachment를 생성하는 리소스입니다. Secret Attachment는 시크릿을 타깃 리소스에 연결합니다. 여기서는 앞서 생성한 Orders 서비스 DB 클러스터용 DB Secret을 Orders 서비스 DB 클러스터에 연결하여, 해당 시크릿이 이 DB 클러스터의 Credentials라는 관계를 명시합니다.

---

`"SecretId"`는 앞서 생성한 DB 시크릿을 지정하고, `"TargetId"`는 Orders 서비스 DB 클러스터를 지정하여 이 둘을 연결합니다.

---

`"TargetType": "AWS::RDS::DBCluster"`는 연결 대상인 타깃 리소스가 DB 클러스터 타입임을 의미합니다.

---

## 5. `"NetworkCatalogDBCredentialsSecret3F8AA865"`

```json
  "NetworkCatalogDBCredentialsSecret3F8AA865": {
   "Type": "AWS::SecretsManager::Secret",
   "Properties": {
    "GenerateSecretString": {
     "ExcludePunctuation": true,
     "GenerateStringKey": "password",
     "IncludeSpace": false,
     "PasswordLength": 10,
     "SecretStringTemplate": "{\"username\":\"root\"}"
    },
    "KmsKeyId": {
     "Fn::GetAtt": [
      "NetworkKMSKey37A7DAB2",
      "Arn"
     ]
    },
    "Name": "retail-store-ecs-catalog-db",
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

`AWS Secrets Manager`에 저장되는 Secret을 생성하는 리소스입니다. 이 시크릿은 Catalog 서비스 DB 클러스터용 DB Secret입니다.

---

`"SecretStringTemplate": "{\"username\":\"root\"}"`를 보면 사용자 이름으로 `"root"`가 지정되어 있습니다. MySQL 계열에서는 기본 관리자 사용자 이름으로 `"root"`를 주로 사용합니다.

---

## 6. `"NetworkCatalogDBCredentialsSecretAttachment39F70EC9"`

```json
  "NetworkCatalogDBCredentialsSecretAttachment39F70EC9": {
   "Type": "AWS::SecretsManager::SecretTargetAttachment",
   "Properties": {
    "SecretId": {
     "Ref": "NetworkCatalogDBCredentialsSecret3F8AA865"
    },
    "TargetId": {
     "Ref": "NetworkCatalogClusterB0717C37"
    },
    "TargetType": "AWS::RDS::DBCluster"
   }
  }
```

Secret Attachment를 생성하는 리소스입니다. Secret Attachment는 시크릿을 타깃 리소스에 연결합니다. 여기서는 앞서 생성한 Catalog 서비스 DB 클러스터용 DB Secret을 Catalog 서비스 DB 클러스터에 연결하여, 해당 시크릿이 이 DB 클러스터의 Credentials라는 관계를 명시합니다.

