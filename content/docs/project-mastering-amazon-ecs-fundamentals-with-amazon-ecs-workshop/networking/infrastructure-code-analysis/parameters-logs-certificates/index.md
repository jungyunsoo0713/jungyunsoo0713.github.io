---
title: "06. 파라미터, 로그 및 인증서 (SSM, Logs, ACM PCA)"
weight: 6
---
> **작성일:** 2026-05-06 | **수정일:** 2026-05-06
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
