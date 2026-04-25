---
title: "04. 암호화 및 시크릿 (KMS & Secrets Manager)"
weight: 4
---
> **작성일:** 2026-04-25 | **수정일:** 2026-04-25
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

`"NetworkKMSKey37A7DAB2"`는 AWS KMS(Key Management Service)의 KMS Key를 생성하는 리소스입니다. 이 키는 DB Password 같은 민감한 값을 암호화하고 복호화하는 데 사용됩니다.

애플리케이션이나 AWS 서비스는 민감한 데이터를 평문으로 저장하지 않습니다. 대신 KMS Key로 보호되는, 즉 KMS Key로 암호화된 데이터 키를 사용해 데이터를 암호화합니다. 실제 데이터를 암호화하는 것은 데이터 키이고, KMS Key는 이 데이터 키를 암호화해 안전하게 보관할 수 있도록 돕는 상위 키입니다.

일반적으로 AWS 서비스는 KMS를 통해 데이터 키를 발급받고, 그 데이터 키로 실제 데이터를 암호화합니다. 이후 데이터 키 자체도 KMS Key로 암호화된 상태로 함께 저장됩니다. 데이터를 다시 읽어야 할 때는 암호화된 데이터 키를 KMS에 전달하고, KMS는 자신의 KMS Key를 사용해 데이터 키를 복호화합니다. AWS 서비스는 복호화된 데이터 키를 사용해 원래 데이터를 복호화합니다.

이 템플릿에서는 Secrets Manager에 저장되는 DB 접속 정보가 `"NetworkKMSKey37A7DAB2"`를 사용해 보호됩니다.

참고로 AWS Secrets Manager는 비밀번호, API Key, 인증 토큰처럼 외부에 노출되면 안 되는 민감한 값을 안전하게 저장하고 관리하기 위한 서비스입니다.

---

`"Description": "retail store ecs CMK"`는 이 KMS Key에 대한 설명입니다. 여기서 `CMK`는 Customer Managed Key를 의미하며, AWS가 자동으로 제공하는 기본 관리형 키가 아니라 사용자가 직접 생성하고 관리하는 KMS Key를 뜻합니다. 이 템플릿에서는 `retail-store-ecs` 애플리케이션에서 사용하는 민감한 값을 암호화하기 위한 키라는 의미로 볼 수 있습니다.

---

`"EnableKeyRotation": true`는 이 KMS Key의 자동 키 교체를 활성화한다는 뜻입니다.

KMS Key는 민감한 데이터를 암호화하는 데 사용되므로, 보안을 위해 키의 내부 암호화 재료를 주기적으로 교체할 수 있습니다. 이 설정이 `true`이면 AWS KMS가 일정 주기마다 키 재료를 자동으로 교체합니다.

---

`"KeyPolicy"`는 이 KMS Key를 누가 관리하고 사용할 수 있는지 정하는 정책입니다. 

---

```json
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
```

이 Policy의 규칙을 정의합니다.  

`"Action": "kms:*"`은 KMS에서 할 수 있는 모든 작업을 허용 대상으로 지정한다는 의미입니다.

`"Principal"`은 이 KMS Key에 대한 권한을 받을 주체를 지정합니다. 여기서는 AWS 계정의 root principal을 지정합니다. 이 root principal은 특정 IAM Role 하나가 아니라, 현재 AWS 계정 자체를 대표하는 주체라고 볼 수 있습니다.

KMS Key는 IAM 권한뿐만 아니라 Key Policy의 영향도 받습니다. 따라서 Key Policy에서 현재 계정에 대한 기본 권한을 열어두어야, 이후 해당 계정의 IAM 사용자나 Role이 이 KMS Key를 관리하거나 사용할 수 있습니다.

KMS Key Policy는 특정 KMS Key 안에 포함된 정책이기 때문에, `"Resource": "*"`는 모든 KMS Key를 의미한다기보다 이 정책이 연결된 해당 KMS Key를 대상으로 한다는 의미입니다.

---

```json
       "Action": [
        "kms:CreateGrant",
        "kms:Decrypt",
        "kms:DescribeKey",
        "kms:Encrypt",
        "kms:GenerateDataKey*",
        "kms:ReEncrypt*"
       ],
```

두 번째 규칙의 `"Action"`은 이 KMS Key에 대해 허용할 KMS 작업 목록입니다. 여기에는 암호화(`"kms:Encrypt"`), 복호화(`"kms:Decrypt"`), 키 정보 조회(`"kms:DescribeKey"`), 데이터 키 생성(`"kms:GenerateDataKey*"`), 재암호화(`"kms:ReEncrypt*"`), Grant 생성 작업(`"kms:CreateGrant"`)이 포함됩니다. `*`는 와일드카드입니다. 따라서 각각 `kms:GenerateDataKey` 또는 `kms:ReEncrypt`로 시작하는 여러 KMS 작업을 함께 포함합니다.

---

```json
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
```

`"Condition"`은 이 권한 규칙(두 번째 `"Statement"`)이 적용되기 위한 적용 조건을 의미합니다.

`"StringEquals"`는 조건 값을 문자열로 비교했을 때, 지정한 값과 정확히 일치해야 한다는 의미입니다.

`"kms:ViaService"`는 KMS 요청이 특정 AWS 서비스를 통해 들어온 경우에만 조건을 만족하도록 제한하는 조건 키입니다. 이 값은 대략`"secretsmanager.<현재 리전>.amazonaws.com"` 이런 형태를 가집니다. 즉 KMS 요청이 현재 리전의 Secrets Manager를 통해 들어온 경우에만 이 조건을 만족한다는 뜻입니다.

---

이 `"Principal"`은 앞의 첫 번째 `"Statement"`에 나온 `"Principal"`과 같은 값입니다. 여기서도 `arn:aws:iam::<현재 AWS 계정 ID>:root` 형태의 root principal을 지정합니다.

---

`"PendingWindowInDays": 7`은 이 KMS Key를 삭제할 때 바로 삭제하지 않고, 7일 동안 삭제 대기 상태로 둔다는 의미입니다. KMS Key는 삭제되면 해당 키로 암호화된 데이터를 복호화할 수 없기 때문에, 삭제 전에 대기 기간을 둡니다.

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

앞에서 생성한 KMS Key에 Alias(별칭)를 붙이는 리소스입니다. 

---

`"AliasName": "alias/retail-store-ecs-cmk"`는 이 KMS Key를 `alias/retail-store-ecs-cmk`라는 이름으로 참조할 수 있게합니다

---

`"TargetKeyId"`는 이 Alias가 `"TargetKeyId"`는 이 Alias가 가리킬 KMS Key를 지정하는 값입니다. 여기서는`"NetworkKMSKey37A7DAB2"` KMS Key를 가리키도록 설정합니다.

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

AWS Secrets Manager에 DB 접속 정보(`username`/`password`)를 저장하는 Secret을 생성하는 리소스입니다.

---

```json
    "GenerateSecretString": {
     "ExcludePunctuation": true,
     "GenerateStringKey": "password",
     "IncludeSpace": false,
     "PasswordLength": 10,
     "SecretStringTemplate": "{\"username\":\"postgres\"}"
    },
```

`"GenerateSecretString"`은 Secret에 저장할 값을 자동으로 생성하는 설정입니다. 일반적으로 이 값은 사용자가 직접 입력하는 것이 아니라, 사용자가 지정한 규칙(예: `"PasswordLength": 10`, `"ExcludePunctuation": true` 등) 에 따라 AWS Secrets Manager가 자동으로 생성합니다.

`"GenerateStringKey": "password"`는 자동으로 생성된 문자열 값을 Secret JSON 안의 `"password"` 필드에 저장하겠다는 의미입니다.

`"SecretStringTemplate": "{\"username\":\"postgres\"}"`는 Secret에 기본으로 들어갈 JSON 문자열을 지정합니다. 여기서는 `"username"` 값을 `"postgres"`로 미리 넣어둡니다.

---

```json
    "KmsKeyId": {
     "Fn::GetAtt": [
      "NetworkKMSKey37A7DAB2",
      "Arn"
     ]
    },
```

`"KmsKeyId"`는 이 Secret 값을 보호할 때 사용할 KMS Key를 지정합니다. 여기서는 `"NetworkKMSKey37A7DAB2"`의 ARN을 가져와, Secrets Manager가 해당 KMS Key를 사용해 Secret 값을 암호화하도록 설정합니다.

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

Secrets Manager의 Secret을 RDS DB Cluster에 연결하는 리소스입니다. 

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

AWS Secrets Manager에 DB 접속 정보(`username`/`password`)를 저장하는 Secret을 생성하는 리소스입니다. `"NetworkDBCredentialsSecretADE5ABB3"`와 동일한 구조를 가지고 있습니다. 다만 이 Secret은 `"username"` 값이 `"root"`로 설정되어 있고, Secret 이름은 `"retail-store-ecs-catalog-db"`입니다.

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

`"NetworkDBCredentialsSecretAttachment41F10924"`와 마찬가지로 Secrets Manager의 Secret을 RDS DB Cluster에 연결하는 리소스입니다. 
