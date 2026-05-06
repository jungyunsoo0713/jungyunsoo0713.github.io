---
title: "06. 파라미터, 로그 및 인증서 (SSM, Logs, ACM PCA)"
weight: 6
---
> 	**작성일:** 2026-05-06 | **수정일:** 2026-05-06
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

Parameter를 생성하는 리소스입니다. 생성된 파라미터는 AWS Systems Manager Parameter Store에 저장됩니다. 이 파라미터는 Orders 서비스 DB 클러스터의 엔드포인트 값에 해당합니다.

---

`"Name": "/retail-store-ecs/orders/db-endpoint-postgres"`는 파라미터의 이름을 뜻합니다.

---

`"Type": "String"`은 파라미터의 값이 문자열 타입이라는 것을 의미합니다. 이 필드에는 `"StringList"`도 지정할 수 있습니다. `"StringList"`는 여러 개의 문자열 값을 쉼표로 구분해 저장하는 타입입니다. 즉, 하나의 파라미터에 여러 개의 문자열 값을 저장하는 타입입니다. 

---

`"Value"`는 이 파라미터의 실제 값을 의미합니다. Orders 서비스 DB 클러스터의 주소(Address) 부분과 포트(Port) 부분을 연결해서 이 DB 엔드포인트 값을 생성합니다.

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

Parameter를 생성하는 리소스입니다. 생성된 파라미터는 AWS Systems Manager Parameter Store에 저장됩니다. 이 파라미터는 Redis Cache 클러스터의 엔드포인트 값에 해당합니다.

`"rediss://"`는 Redis Cache에 TLS로 접속한다는 의미입니다. Redis Cache 엔드포인트가 이 문자열로 시작하기 때문에, Redis Cache에 접속할 때 TLS 암호화 연결이 이루어집니다.

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

Parameter를 생성하는 리소스입니다. 생성된 파라미터는 AWS Systems Manager Parameter Store에 저장됩니다. 이 파라미터는 Catalog 서비스 DB 클러스터의 엔드포인트 값에 해당합니다.

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

로그 그룹을 생성하는 리소스입니다. 이 로그 그룹은 ECS Service Connect 로그를 저장하기 위한 로그 그룹입니다.

---

`"LogGroupName": "/aws/ecs/service-connect/retail-store"`는 이 로그 그룹의 이름을 의미합니다.

---

`"RetentionInDays": 7`은 이 로그 그룹에 있는 로그가 발생 후 7일 동안 저장된다는 것을 의미합니다. 7일이 지난 로그는 삭제됩니다.

---

## 5. `NetworkECSImmersionDayCertificateAthority093C06DF`

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

Certificate Authority(CA)를 생성하는 리소스입니다. CA는 CA 인증서와 일반 인증서를 발급할 수 있는 주체입니다.

---

`"KeyAlgorithm": "RSA_2048"`는 이 CA 키 쌍의 알고리즘입니다. 이 CA는 개인 키로 CSR에 서명하고, 이후 인증서에 서명할 때도 개인 키를 사용합니다. 검증하는 주체는 이 CA의 공개 키로 서명을 검증합니다. 인증서를 검증하는 주체는 클라이언트나 VPC 내부의 서버 등이 있습니다.

---

이 리소스 타입(`"AWS::ACMPCA::CertificateAuthority"`)에서 `"SigningAlgorithm": "SHA256WITHRSA"`는 CA가 CSR(Certificate Signing Request)에 서명할 때 사용하는 서명 방식입니다.

CA 인증서를 발급하려면 CSR이라는 인증서 발급 신청서가 필요합니다. 다음은 CA 생성 절차를 포함한 CA 인증서 발급 절차입니다. CA 키 쌍 생성 → CSR 내용 구성 → CA 개인 키로 CSR에 서명 → CSR 생성 완료 → CA 리소스 생성 완료 → CSR의 정보를 바탕으로 CA 인증서 생성 → CA 인증서에 서명 → CA 인증서 발급 순으로 인증서 최종 발급이 이루어집니다.

다음은 일반 인증서의 발급 절차입니다. 서버/클라이언트 키 쌍 생성 → CSR 내용 구성 → 서버/클라이언트 개인 키로 CSR에 서명 → CSR 생성 완료 → CA가 CSR의 정보를 바탕으로 일반 인증서 생성 → CA가 일반 인증서에 서명 → 일반 인증서 발급

주의할 점으로는 CSR에 서명하는 경우는 CSR을 만드는 쪽(CA/서버/클라이언트)이 자신의 개인 키로 서명합니다.

비유를 통해서 말하자면, `"KeyAlgorithm"`이 서명 시 사용되는 펜이라면, `"SigningAlgorithm"`은 서명 전에 문서를 어떻게 요약하고, 어떻게 서명할지를 정하는 서명 방식입니다. 간단히 서명에 쓰이는 펜과 서명 방식이라고 생각하면 됩니다.

주의할 점으로, 서명은 암호화 알고리즘을 사용하지만 데이터의 기밀성을 위한 것이 아니라, 데이터의 무결성과 출처 확인을 위한 것이라는 점입니다.

---

`"Subject"`는 이 CA의 주체 정보(이 CA가 누구인지에 대한 정보)를 가지고 있습니다. `"Subject"`에는 정해져 있는 속성도 있고, 사용자가 직접 정해서 넣을 수 있는 속성도 있습니다. 필수 속성은 존재하지 않습니다.

---

`"Type": "ROOT"`는 이 CA를 루트 CA로 만들겠다는 의미입니다. 루트 CA는 다른 CA에게 CA 인증서를 발급받지 않는 CA로, 인증서 신뢰 체계의 최상위 CA를 의미합니다.

---

`"UsageMode": "SHORT_LIVED_CERTIFICATE"`는 이 CA가 유효기간이 상대적으로 짧은 인증서를 발급하는 모드라는 의미입니다. 이때 인증서는 CA 인증서만이 아닌 모든 인증서를 가리킵니다.

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

CA가 발급하는 인증서를 생성하는 리소스입니다. 세부 설정에 따라 CA 인증서를 발급할지, 일반 인증서를 발급할지가 정해집니다.

---

`"CertificateAuthorityArn"`는 이 인증서를 발급하는 CA를 지정하는 필드입니다. 앞서 생성한 CA의 ARN을 지정합니다.

---

`"CertificateSigningRequest"`는 CSR을 의미합니다. CSR은 인증서를 발급할 때 포함할 정보를 담고 있습니다. 여기서는 앞서 생성한 CA의 `"CertificateSigningRequest"`를 지정합니다. `"CertificateSigningRequest"`는 이 CloudFormation 템플릿에 직접 작성된 값이 아니라, CA 생성 시 생성되는 속성값입니다.

---

이 리소스 타입(`"AWS::ACMPCA::Certificate"`)에서 `"SigningAlgorithm": "SHA256WITHRSA"`는 CA가 이 인증서에 서명할 때 사용하는 서명 방식을 의미합니다. 이때는 CA가 CSR에 사용하는 서명 방식이 아니라, 인증서에 사용하는 서명 방식입니다.

---

`"TemplateArn"`은 인증서 템플릿을 지정하는 필드입니다. 값으로 `"arn:aws:acm-pca:::template/RootCACertificate/V1"`을 사용합니다. 이 템플릿은 루트 CA 인증서를 발급하는 템플릿입니다. 즉, 이 리소스는 CA 인증서를 발급합니다. 따라서 이 필드가 어떤 인증서 종류를 발급할지 정하는 역할도 합니다.

---

`"Validity"`는 인증서의 유효기간을 정합니다. `"Type": "YEARS"`와 `"Value": 2`는 유효기간이 2년이라는 의미입니다.

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

Certificate Authority Activation을 생성하는 리소스입니다. 이 리소스는 인증서를 CA에 연결하여 CA를 활성화하는 리소스입니다. 앞서 CA와 CA 인증서를 생성했기 때문에 이 둘을 연결합니다.

