---
title: "06. 파라미터, 로그 및 인증서 (SSM, Logs, ACM PCA)"
weight: 6
---
> **작성일:** 2026-04-26 | **수정일:** 2026-04-26
```json
{
  "NetworkDBEndpointParameter9BA21FDB": {
   "Type": "AWS::SSM::Parameter",
   "Properties": {
    "Name": "/retail-store-ecs/orders/db-endpoint-postgres",
    "Tags": {
     "CreationDate": "2026-03-31",
     "Version": "1.0.4",
     "WorkshopName": "ECSImmersionDay"
    },
    "Type": "String",
    "Value": {
     "Fn::Join": [
      ":",
      [
       {
        "Fn::GetAtt": [
         "NetworkOrdersCluster095F1433",
         "Endpoint.Address"
        ]
       },
       {
        "Fn::GetAtt": [
         "NetworkOrdersCluster095F1433",
         "Endpoint.Port"
        ]
       }
      ]
     ]
    }
   }
  },
  "NetworkRedisEndpointParameter558525EA": {
   "Type": "AWS::SSM::Parameter",
   "Properties": {
    "Name": "/retail-store-ecs/checkout/redis-endpoint",
    "Tags": {
     "CreationDate": "2026-03-31",
     "Version": "1.0.4",
     "WorkshopName": "ECSImmersionDay"
    },
    "Type": "String",
    "Value": {
     "Fn::Join": [
      "",
      [
       "rediss://",
       {
        "Fn::Join": [
         ":",
         [
          {
           "Fn::GetAtt": [
            "NetworkRedisCluster8A158537",
            "Endpoint.Address"
           ]
          },
          {
           "Fn::GetAtt": [
            "NetworkRedisCluster8A158537",
            "Endpoint.Port"
           ]
          }
         ]
        ]
       }
      ]
     ]
    }
   }
  },
  "NetworkCatalogDBEndpointParameterF3C95C74": {
   "Type": "AWS::SSM::Parameter",
   "Properties": {
    "Name": "/retail-store-ecs/catalog/db-endpoint-mysql",
    "Tags": {
     "CreationDate": "2026-03-31",
     "Version": "1.0.4",
     "WorkshopName": "ECSImmersionDay"
    },
    "Type": "String",
    "Value": {
     "Fn::Join": [
      ":",
      [
       {
        "Fn::GetAtt": [
         "NetworkCatalogClusterB0717C37",
         "Endpoint.Address"
        ]
       },
       {
        "Fn::GetAtt": [
         "NetworkCatalogClusterB0717C37",
         "Endpoint.Port"
        ]
       }
      ]
     ]
    }
   }
  },
  "NetworkServiceConnectLogGroupB173FC23": {
   "Type": "AWS::Logs::LogGroup",
   "Properties": {
    "LogGroupName": "/aws/ecs/service-connect/retail-store",
    "RetentionInDays": 7,
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
  "NetworkECSImmersionDayCertificateAthority093C06DF": {
   "Type": "AWS::ACMPCA::CertificateAuthority",
   "Properties": {
    "KeyAlgorithm": "RSA_2048",
    "SigningAlgorithm": "SHA256WITHRSA",
    "Subject": {
     "Country": "US",
     "Organization": "ECS Immersion Day",
     "OrganizationalUnit": "Workshop"
    },
    "Tags": [
     {
      "Key": "AmazonECSManaged",
      "Value": "true"
     },
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
    "Type": "ROOT",
    "UsageMode": "SHORT_LIVED_CERTIFICATE"
   }
  },
  "NetworkECSImmersionDayCertificateAthorityCertificate639EED5C": {
   "Type": "AWS::ACMPCA::Certificate",
   "Properties": {
    "CertificateAuthorityArn": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthority093C06DF",
      "Arn"
     ]
    },
    "CertificateSigningRequest": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthority093C06DF",
      "CertificateSigningRequest"
     ]
    },
    "SigningAlgorithm": "SHA256WITHRSA",
    "TemplateArn": "arn:aws:acm-pca:::template/RootCACertificate/V1",
    "Validity": {
     "Type": "YEARS",
     "Value": 2
    }
   }
  },
  "NetworkECSImmersionDayCertificateAthorityActivationCDA9039C": {
   "Type": "AWS::ACMPCA::CertificateAuthorityActivation",
   "Properties": {
    "Certificate": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthorityCertificate639EED5C",
      "Certificate"
     ]
    },
    "CertificateAuthorityArn": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthority093C06DF",
      "Arn"
     ]
    }
   }
  }
}
```

---
## 1. `"NetworkDBEndpointParameter9BA21FDB"`

```json
  "NetworkDBEndpointParameter9BA21FDB": {
   "Type": "AWS::SSM::Parameter",
   "Properties": {
    "Name": "/retail-store-ecs/orders/db-endpoint-postgres",
    "Tags": {
     "CreationDate": "2026-03-31",
     "Version": "1.0.4",
     "WorkshopName": "ECSImmersionDay"
    },
    "Type": "String",
    "Value": {
     "Fn::Join": [
      ":",
      [
       {
        "Fn::GetAtt": [
         "NetworkOrdersCluster095F1433",
         "Endpoint.Address"
        ]
       },
       {
        "Fn::GetAtt": [
         "NetworkOrdersCluster095F1433",
         "Endpoint.Port"
        ]
       }
      ]
     ]
    }
   }
  },
```

Orders 서비스의 SSM Parameter 리소스입니다. Orders 서비스가 사용할 DB 엔드포인트 값을 SSM Parameter Store에 저장합니다.

---

`"Name": "/retail-store-ecs/orders/db-endpoint-postgres"`은 이 SSM Parameter 에 저장될 DB 엔드포인트의 이름을 의미합니다. 

---

`"Type": "String"`은 SSM Parameter Store에 저장할 값의 타입이 문자열이라는것을 알려줍니다.

---

```json
    "Value": {
     "Fn::Join": [
      ":",
      [
       {
        "Fn::GetAtt": [
         "NetworkOrdersCluster095F1433",
         "Endpoint.Address"
        ]
       },
       {
        "Fn::GetAtt": [
         "NetworkOrdersCluster095F1433",
         "Endpoint.Port"
        ]
       }
      ]
     ]
    }
```

SSM Parameter에 저장되는 DB 엔드포인트 값을 의미합니다.

`NetworkOrdersCluster095F1433` Aurora PostgreSQL 클러스터의 엔드포인트 주소와 포트 번호를 `:`로 합쳐 하나의 문자열로 만듭니다. 예를 들어 결과 값은 `orders-cluster.xxxxxx.rds.amazonaws.com:5432` 같은 형태가 됩니다.

---
## 2. `"NetworkRedisEndpointParameter558525EA"`

```json
  "NetworkRedisEndpointParameter558525EA": {
   "Type": "AWS::SSM::Parameter",
   "Properties": {
    "Name": "/retail-store-ecs/checkout/redis-endpoint",
    "Tags": {
     "CreationDate": "2026-03-31",
     "Version": "1.0.4",
     "WorkshopName": "ECSImmersionDay"
    },
    "Type": "String",
    "Value": {
     "Fn::Join": [
      "",
      [
       "rediss://",
       {
        "Fn::Join": [
         ":",
         [
          {
           "Fn::GetAtt": [
            "NetworkRedisCluster8A158537",
            "Endpoint.Address"
           ]
          },
          {
           "Fn::GetAtt": [
            "NetworkRedisCluster8A158537",
            "Endpoint.Port"
           ]
          }
         ]
        ]
       }
      ]
     ]
    }
   }
  },
```

Checkout 서비스가 Redis에 접속할 때 사용할 Redis 엔드포인트 값을 SSM Parameter Store에 저장하는 리소스입니다. 

구조는 `"NetworkDBEndpointParameter9BA21FDB"`와 거의 같지만, Orders DB 엔드포인트가 아니라 Checkout 서비스용 Redis 엔드포인트를 저장한다는 점이 다릅니다. 

---

`"rediss://"`는 TLS/SSL 기반 Redis 접속을 의미하는 일반적인 URI 접두사입니다. 일반 Redis 접속에는 보통 `"redis://"`를 사용하고, 암호화된 Redis 접속에는 `"rediss://"`를 사용합니다. 이 템플릿에서는 Checkout 서비스가 Redis에 TLS 기반으로 접속하도록 Redis 엔드포인트 값 앞에 `"rediss://"`를 붙입니다.

---
## 3. `"NetworkCatalogDBEndpointParameterF3C95C74"`

```json
  "NetworkCatalogDBEndpointParameterF3C95C74": {
   "Type": "AWS::SSM::Parameter",
   "Properties": {
    "Name": "/retail-store-ecs/catalog/db-endpoint-mysql",
    "Tags": {
     "CreationDate": "2026-03-31",
     "Version": "1.0.4",
     "WorkshopName": "ECSImmersionDay"
    },
    "Type": "String",
    "Value": {
     "Fn::Join": [
      ":",
      [
       {
        "Fn::GetAtt": [
         "NetworkCatalogClusterB0717C37",
         "Endpoint.Address"
        ]
       },
       {
        "Fn::GetAtt": [
         "NetworkCatalogClusterB0717C37",
         "Endpoint.Port"
        ]
       }
      ]
     ]
    }
   }
  },
```

Catalog 서비스가 MySQL DB에 접속할 때 사용할 DB 엔드포인트 값을 SSM Parameter Store에 저장하는 리소스입니다. 

구조는 앞에서 본 `"NetworkDBEndpointParameter9BA21FDB"`와 거의 같습니다. 다만 Orders 서비스용 PostgreSQL 엔드포인트가 아니라, Catalog 서비스용 MySQL 엔드포인트를 저장한다는 점이 다릅니다.

---
## 4. `"NetworkServiceConnectLogGroupB173FC23"`

```json
  "NetworkServiceConnectLogGroupB173FC23": {
   "Type": "AWS::Logs::LogGroup",
   "Properties": {
    "LogGroupName": "/aws/ecs/service-connect/retail-store",
    "RetentionInDays": 7,
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

ECS Service Connect 로그를 저장하기 위한 CloudWatch Logs Log Group을 생성합니다.

---
## 5. `"NetworkECSImmersionDayCertificateAthority093C06DF"`

```json
  "NetworkECSImmersionDayCertificateAthority093C06DF": {
   "Type": "AWS::ACMPCA::CertificateAuthority",
   "Properties": {
    "KeyAlgorithm": "RSA_2048",
    "SigningAlgorithm": "SHA256WITHRSA",
    "Subject": {
     "Country": "US",
     "Organization": "ECS Immersion Day",
     "OrganizationalUnit": "Workshop"
    },
    "Tags": [
     {
      "Key": "AmazonECSManaged",
      "Value": "true"
     },
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
    "Type": "ROOT",
    "UsageMode": "SHORT_LIVED_CERTIFICATE"
   }
  },
```

ECS Service Connect의 TLS 통신에 사용할 AWS Private CA를 생성합니다. 이 Private CA는 Service Connect에서 서비스 간 통신을 암호화할 때 필요한 인증서를 발급하는 역할을 합니다.

---

`"KeyAlgorithm": "RSA_2048"`와 `"SigningAlgorithm": "SHA256WITHRSA"`는 인증서 발급과 서명에 사용할 암호화 알고리즘을 지정합니다.

---

`"Type": "ROOT"`는 이 CA를 Root CA로 생성함을 의미합니다. Root CA는 다른 CA의 인증을 받는 하위 인증 기관이 아니라, 스스로 신뢰의 기준이 되는 최상위 인증 기관입니다. 이 Root CA가 발급하거나 서명한 인증서는 해당 Root CA를 신뢰하는 환경 안에서 유효한 인증서로 인정됩니다.

---

`"UsageMode": "SHORT_LIVED_CERTIFICATE"`는 이 Private CA가 최대 7일 동안만 유효한 짧은 수명의 인증서를 발급하는 용도로 사용된다는 의미입니다. 이런 인증서는 유효 기간이 짧기 때문에, 오래 쓰는 인증서보다 자주 발급되고 교체되는 구조에 적합합니다.

---
## 6. `"NetworkECSImmersionDayCertificateAthorityCertificate639EED5C"`

```json
  "NetworkECSImmersionDayCertificateAthorityCertificate639EED5C": {
   "Type": "AWS::ACMPCA::Certificate",
   "Properties": {
    "CertificateAuthorityArn": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthority093C06DF",
      "Arn"
     ]
    },
    "CertificateSigningRequest": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthority093C06DF",
      "CertificateSigningRequest"
     ]
    },
    "SigningAlgorithm": "SHA256WITHRSA",
    "TemplateArn": "arn:aws:acm-pca:::template/RootCACertificate/V1",
    "Validity": {
     "Type": "YEARS",
     "Value": 2
    }
   }
  },
```

Private CA를 Root CA로 활성화하기 위해 필요한 CA 인증서를 발급하는 리소스입니다.

---

`"CertificateAuthorityArn"`은 인증서를 발급할 Private CA를 지정합니다.

---

`"CertificateSigningRequest"`는 해당 CA의 인증서 서명 요청을 가져옵니다. 

>`"CertificateSigningRequest"`는 `"NetworkECSImmersionDayCertificateAthority093C06DF"`의 `"Properties"`에 직접 적힌 값이 아니라, `"NetworkECSImmersionDayCertificateAthority093C06DF"` Private CA 리소스가 생성될 때 AWS Private CA가 자동으로 만들어주는 속성값입니다. CloudFormation은 `Fn::GetAtt`을 사용해 이 CSR 값을 가져오고, 이를 Root CA 인증서 발급에 사용합니다.

---

`"TemplateArn"`에는 Root CA 인증서 템플릿인 `"RootCACertificate/V1"`이 사용됩니다. 이는 이 인증서를 일반 서버 인증서가 아니라, Root CA 자신을 증명하는 Root CA 인증서로 발급하겠다는 의미입니다. 즉, 앞에서 생성한 Private CA가 인증 체인의 최상위 인증 기관으로 동작할 수 있도록, 해당 CA용 인증서를 발급하는 설정입니다.

---

`"Validity"`는 이 Root CA 인증서의 유효 기간을 2년으로 설정합니다. 

>Root CA 인증서는 Root CA 자신을 증명하는 인증서입니다. 이 인증서는 해당 CA가 인증 체인의 최상위 인증 기관으로 동작할 수 있게 해주며, Root CA가 발급하거나 서명한 인증서를 신뢰할 수 있는 기준점 역할을 합니다.

>여기서 `"Validity"`는 이 Root CA 인증서 자체의 유효 기간을 2년으로 설정합니다. 반면 앞에서 본 `"UsageMode": "SHORT_LIVED_CERTIFICATE"`는 이 CA가 앞으로 발급할 서비스용 인증서의 수명을 짧게 제한한다는 의미입니다. 즉, Root CA 인증서 자체는 2년 동안 유효하지만, 이 CA가 발급하는 Service Connect용 인증서는 최대 7일처럼 짧은 수명을 가질 수 있습니다.

>요약하자면, Root CA, 즉 인증서를 발급하는 인증 기관 자체는 2년 동안 유효하고, 이 CA가 발급하는 Service Connect용 짧은 수명의 인증서는 최대 7일 동안 유효합니다.

---
## 7. `"NetworkECSImmersionDayCertificateAthorityActivationCDA9039C"`

```json
  "NetworkECSImmersionDayCertificateAthorityActivationCDA9039C": {
   "Type": "AWS::ACMPCA::CertificateAuthorityActivation",
   "Properties": {
    "Certificate": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthorityCertificate639EED5C",
      "Certificate"
     ]
    },
    "CertificateAuthorityArn": {
     "Fn::GetAtt": [
      "NetworkECSImmersionDayCertificateAthority093C06DF",
      "Arn"
     ]
    }
   }
  }
```

생성한 Private CA를 활성화하는 리소스입니다. 

---

`"Certificate"`에 발급한 Root CA 인증서를 넣습니다.

---

`"CertificateAuthorityArn"`에는 활성화할 Private CA의 ARN을 지정합니다. 

즉, 이 리소스는 생성된 Private CA와 Root CA 인증서를 연결해, 해당 CA가 실제로 인증서를 발급할 수 있는 상태가 되도록 만드는 역할을 합니다.
