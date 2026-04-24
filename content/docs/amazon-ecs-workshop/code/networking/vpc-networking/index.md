---
title: "01. VPC 및 네트워크 인프라 (VPC, Subnets, Gateways, Endpoints, DNS"
weight: 1
---
> **작성일:** 2026-04-24 | **수정일:** 2026-04-24
```json
{
  "NetworkRetailStoreDnsNamespace0ED145E4": {
   "Type": "AWS::ServiceDiscovery::PrivateDnsNamespace",
   "Properties": {
    "Description": "Service discovery namespace",
    "Name": "retailstore.local",
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
    "Vpc": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkTargetGroupTLS7EEC93B6": {
   "Type": "AWS::ElasticLoadBalancingV2::TargetGroup",
   "Properties": {
    "HealthCheckIntervalSeconds": 10,
    "HealthCheckPath": "/actuator/health",
    "HealthCheckPort": "8080",
    "HealthCheckTimeoutSeconds": 5,
    "HealthyThresholdCount": 2,
    "Name": "ui-application-tls",
    "Port": 8080,
    "Protocol": "HTTPS",
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
    "TargetGroupAttributes": [
     {
      "Key": "stickiness.enabled",
      "Value": "true"
     },
     {
      "Key": "stickiness.type",
      "Value": "lb_cookie"
     },
     {
      "Key": "stickiness.lb_cookie.duration_seconds",
      "Value": "300"
     }
    ],
    "TargetType": "ip",
    "UnhealthyThresholdCount": 3,
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "NetworkRetailStoreVPC2A069619D": {
   "Type": "AWS::EC2::VPC",
   "Properties": {
    "CidrBlock": "192.168.0.0/16",
    "EnableDnsHostnames": true,
    "EnableDnsSupport": true,
    "InstanceTenancy": "default",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "lattice-retail-store-vpc"
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
  "NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3": {
   "Type": "AWS::EC2::Subnet",
   "Properties": {
    "AvailabilityZone": {
     "Fn::Sub": [
      "${Region}a",
      {
       "Region": {
        "Ref": "AWS::Region"
       }
      }
     ]
    },
    "CidrBlock": "192.168.0.0/24",
    "MapPublicIpOnLaunch": true,
    "Tags": [
     {
      "Key": "aws-cdk:subnet-name",
      "Value": "Public"
     },
     {
      "Key": "aws-cdk:subnet-type",
      "Value": "Public"
     },
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PublicSubnet1RouteTableAssociation370D0C06": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426"
    },
    "SubnetId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3"
    }
   }
  },
  "NetworkRetailStoreVPC2PublicSubnet1DefaultRoute3EAAA9F0": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "NetworkRetailStoreVPC2IGW56CDF441"
    },
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426"
    }
   },
   "DependsOn": [
    "NetworkRetailStoreVPC2VPCGW00725957"
   ]
  },
  "NetworkRetailStoreVPC2PublicSubnet1EIP7692665A": {
   "Type": "AWS::EC2::EIP",
   "Properties": {
    "Domain": "vpc",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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
  "NetworkRetailStoreVPC2PublicSubnet1NATGateway83A6036F": {
   "Type": "AWS::EC2::NatGateway",
   "Properties": {
    "AllocationId": {
     "Fn::GetAtt": [
      "NetworkRetailStoreVPC2PublicSubnet1EIP7692665A",
      "AllocationId"
     ]
    },
    "SubnetId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3"
    },
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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
   "DependsOn": [
    "NetworkRetailStoreVPC2PublicSubnet1DefaultRoute3EAAA9F0",
    "NetworkRetailStoreVPC2PublicSubnet1RouteTableAssociation370D0C06"
   ]
  },
  "NetworkRetailStoreVPC2PublicSubnet2SubnetEF1E24D7": {
   "Type": "AWS::EC2::Subnet",
   "Properties": {
    "AvailabilityZone": {
     "Fn::Sub": [
      "${Region}b",
      {
       "Region": {
        "Ref": "AWS::Region"
       }
      }
     ]
    },
    "CidrBlock": "192.168.1.0/24",
    "MapPublicIpOnLaunch": true,
    "Tags": [
     {
      "Key": "aws-cdk:subnet-name",
      "Value": "Public"
     },
     {
      "Key": "aws-cdk:subnet-type",
      "Value": "Public"
     },
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet2"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PublicSubnet2RouteTable6E423581": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet2"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PublicSubnet2RouteTableAssociation250D883C": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet2RouteTable6E423581"
    },
    "SubnetId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet2SubnetEF1E24D7"
    }
   }
  },
  "NetworkRetailStoreVPC2PublicSubnet2DefaultRoute6E9CC3F9": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "NetworkRetailStoreVPC2IGW56CDF441"
    },
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet2RouteTable6E423581"
    }
   },
   "DependsOn": [
    "NetworkRetailStoreVPC2VPCGW00725957"
   ]
  },
  "NetworkRetailStoreVPC2PrivateSubnet1SubnetFD8E08D9": {
   "Type": "AWS::EC2::Subnet",
   "Properties": {
    "AvailabilityZone": {
     "Fn::Sub": [
      "${Region}a",
      {
       "Region": {
        "Ref": "AWS::Region"
       }
      }
     ]
    },
    "CidrBlock": "192.168.2.0/24",
    "MapPublicIpOnLaunch": false,
    "Tags": [
     {
      "Key": "aws-cdk:subnet-name",
      "Value": "Private"
     },
     {
      "Key": "aws-cdk:subnet-type",
      "Value": "Private"
     },
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PrivateSubnet1"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PrivateSubnet1RouteTable38AB705E": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PrivateSubnet1"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PrivateSubnet1RouteTableAssociation935FD149": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PrivateSubnet1RouteTable38AB705E"
    },
    "SubnetId": {
     "Ref": "NetworkRetailStoreVPC2PrivateSubnet1SubnetFD8E08D9"
    }
   }
  },
  "NetworkRetailStoreVPC2PrivateSubnet1DefaultRouteB5FC2E06": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "NatGatewayId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1NATGateway83A6036F"
    },
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PrivateSubnet1RouteTable38AB705E"
    }
   }
  },
  "NetworkRetailStoreVPC2PrivateSubnet2Subnet85F8E521": {
   "Type": "AWS::EC2::Subnet",
   "Properties": {
    "AvailabilityZone": {
     "Fn::Sub": [
      "${Region}b",
      {
       "Region": {
        "Ref": "AWS::Region"
       }
      }
     ]
    },
    "CidrBlock": "192.168.3.0/24",
    "MapPublicIpOnLaunch": false,
    "Tags": [
     {
      "Key": "aws-cdk:subnet-name",
      "Value": "Private"
     },
     {
      "Key": "aws-cdk:subnet-type",
      "Value": "Private"
     },
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PrivateSubnet2"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PrivateSubnet2RouteTableC56B628D": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PrivateSubnet2"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkRetailStoreVPC2PrivateSubnet2RouteTableAssociationFEF66F04": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PrivateSubnet2RouteTableC56B628D"
    },
    "SubnetId": {
     "Ref": "NetworkRetailStoreVPC2PrivateSubnet2Subnet85F8E521"
    }
   }
  },
  "NetworkRetailStoreVPC2PrivateSubnet2DefaultRouteA640F78B": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "NatGatewayId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1NATGateway83A6036F"
    },
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PrivateSubnet2RouteTableC56B628D"
    }
   }
  },
  "NetworkRetailStoreVPC2IGW56CDF441": {
   "Type": "AWS::EC2::InternetGateway",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "lattice-retail-store-vpc"
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
  "NetworkRetailStoreVPC2VPCGW00725957": {
   "Type": "AWS::EC2::VPCGatewayAttachment",
   "Properties": {
    "InternetGatewayId": {
     "Ref": "NetworkRetailStoreVPC2IGW56CDF441"
    },
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
  "NetworkGuardDutyVPCEndpointVPCLattice968F5B1D": {
   "Type": "AWS::EC2::VPCEndpoint",
   "Properties": {
    "PrivateDnsEnabled": true,
    "SecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkGuardDutyEndpointSecurityGroupVPCLatticeEA4520D9",
       "GroupId"
      ]
     }
    ],
    "ServiceName": {
     "Fn::Join": [
      "",
      [
       "com.amazonaws.",
       {
        "Ref": "AWS::Region"
       },
       ".guardduty-data"
      ]
     ]
    },
    "SubnetIds": [
     {
      "Ref": "NetworkRetailStoreVPC2PrivateSubnet1SubnetFD8E08D9"
     },
     {
      "Ref": "NetworkRetailStoreVPC2PrivateSubnet2Subnet85F8E521"
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
    ],
    "VpcEndpointType": "Interface",
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  }
}
```

---
## 1. `"NetworkRetailStoreDnsNamespace0ED145E4"`

```json
  "NetworkRetailStoreDnsNamespace0ED145E4": {
   "Type": "AWS::ServiceDiscovery::PrivateDnsNamespace",
   "Properties": {
    "Description": "Service discovery namespace",
    "Name": "retailstore.local",
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
    "Vpc": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

`"NetworkRetailStoreDnsNamespace0ED145E4"`는 VPC 내부에서 사용할 Private DNS Namespace를 생성하는 Cloud Map 리소스입니다. 이 네임스페이스는 외부 사용자가 접근하기 위한 공개 도메인이 아니라, VPC 내부에 등록된 서비스들이 내부 DNS 이름으로 서로를 찾을 수 있게 해주는 용도입니다.

예를 들어, `catalog` 서비스에 연결된 태스크의 IP 주소가 `10.0.2.15`라고 가정해보겠습니다. 이 IP 주소는 태스크가 새로 실행되거나 서비스가 재배포될 때 바뀔 수 있습니다. 그래서 ECS 서비스는 실행 중인 태스크의 IP를 Cloud Map의 Service Discovery Service에 등록하고, Cloud Map은 이 정보를 내부 DNS 이름으로 조회할 수 있게 해줍니다. DNS Namespace는 이런 서비스 이름들을 담는 내부 DNS 이름 공간입니다. 따라서 `retailstore.local` 네임스페이스 안에 `catalog` 서비스가 등록되면, 다른 서비스는 IP 주소를 직접 알 필요 없이 `catalog.retailstore.local` 같은 이름으로 `catalog` 서비스에 접근할 수 있습니다.

---
## 2. `"NetworkTargetGroupTLS7EEC93B6"`

```json
  "NetworkTargetGroupTLS7EEC93B6": {
   "Type": "AWS::ElasticLoadBalancingV2::TargetGroup",
   "Properties": {
    "HealthCheckIntervalSeconds": 10,
    "HealthCheckPath": "/actuator/health",
    "HealthCheckPort": "8080",
    "HealthCheckTimeoutSeconds": 5,
    "HealthyThresholdCount": 2,
    "Name": "ui-application-tls",
    "Port": 8080,
    "Protocol": "HTTPS",
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
    "TargetGroupAttributes": [
     {
      "Key": "stickiness.enabled",
      "Value": "true"
     },
     {
      "Key": "stickiness.type",
      "Value": "lb_cookie"
     },
     {
      "Key": "stickiness.lb_cookie.duration_seconds",
      "Value": "300"
     }
    ],
    "TargetType": "ip",
    "UnhealthyThresholdCount": 3,
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

`"NetworkTargetGroupTLS7EEC93B6"`는 `HTTPS` 요청을 받을 타깃 그룹을 생성하는 리소스입니다. Fundamentals Lab에서는 `"BasicBlueTargetGroup17B9FA9D"`, `"BasicGreenTargetGroup843DBE73"`이라는 `HTTP` 타깃 그룹을 만들었습니다. 이 타깃 그룹은 Blue/Green 배포를 위한 타깃 그룹으로 ALB에 연결되었습니다. 

`"NetworkTargetGroupTLS7EEC93B6"`는 로드 밸런서가 받은 요청을 UI 애플리케이션 태스크로 전달하기 위한 타깃 그룹입니다. 

---

`"Name": "ui-application-tls"`는 이 타깃 그룹이 UI 애플리케이션 태스크들을 대상으로 하는 TLS용 타깃 그룹임을 나타냅니다.

---

`"Protocol": "HTTPS"`는 ALB가 이 타깃 그룹에 등록된 UI 애플리케이션 태스크로 요청을 전달할 때 `HTTPS` 프로토콜을 사용한다는 의미입니다. 즉, ALB가 받은 요청을 타깃의 `8080` 포트로 전달할 때 암호화된 `HTTPS` 통신을 사용하도록 설정한 것입니다. ALB는 클라이언트의 `HTTPS` 요청을 받은 후 타깃으로 보낼 새로운 `HTTPS` 요청을 만들어 전달할 수도 있고, `HTTP` 요청을 받은 후 타깃으로 보낼 새로운 `HTTPS` 요청을 만들어 전달할 수도 있습니다.

---

```json
    "TargetGroupAttributes": [
     {
      "Key": "stickiness.enabled",
      "Value": "true"
     },
```

Fundamentals Lab에 등장했던 `"BasicGreenTargetGroup843DBE73"`과는 다르게 `"stickiness.enabled"`의 값이 `"true"`입니다. `"BasicGreenTargetGroup843DBE73"`은 업데이트된 태스크를 검증해야 하기 때문에 요청을 고르게 분산할 필요가 있는 반면에, `"NetworkTargetGroupTLS7EEC93B6"`는 일반적인 UI 애플리케이션 요청을 처리하는 타깃 그룹이므로 같은 클라이언트의 요청을 일정 시간 동안 같은 태스크로 보내도록 설정한 것으로 볼 수 있습니다.

---

Fundamentals Lab의 `"BasicGreenTargetGroup843DBE73"`은 이 값이 `2`로 설정되어 있었는데, 이는 Green 타깃 그룹이 새 버전 태스크를 검증하는 용도로 사용되기 때문으로 볼 수 있습니다. 새 버전 태스크에 문제가 있다면 빠르게 감지해야 하므로, Green 타깃 그룹은 더 적은 실패 횟수만으로도 타깃을 비정상 상태로 판단하도록 설정한 것으로 볼 수 있습니다.

---
## 3. `"NetworkRetailStoreVPC2A069619D"`

```json
  "NetworkRetailStoreVPC2A069619D": {
   "Type": "AWS::EC2::VPC",
   "Properties": {
    "CidrBlock": "192.168.0.0/16",
    "EnableDnsHostnames": true,
    "EnableDnsSupport": true,
    "InstanceTenancy": "default",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "lattice-retail-store-vpc"
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

`"NetworkRetailStoreVPC2A069619D"`는 Fundamentals Lab에서 생성했던 VPC와는 다른 VPC입니다. Fundamentals Lab에서 생성했던 VPC의 주소 대역은 `"10.0.0.0/16"`이었고, 이 VPC의 주소 대역은 `"192.168.0.0/16"`입니다. 즉, 서로 다른 주소 대역을 사용하지만 같은 크기의 주소 범위를 가지고 있습니다. 나머지 설정은 거의 동일합니다. 이하 이 VPC를 VPC2로 지칭하겠습니다.

---

`"lattice-retail-store-vpc"`의 이름으로 보아, VPC Lattice 실습용으로 따로 만든 VPC로 추측됩니다.

---

이 VPC2(`NetworkRetailStoreVPC2A069619D"`) 관련 리소스들은은 VPC Lattice 실습을 위한 별도의 네트워크 환경을 구성하는 부분으로 볼 수 있습니다. `"NetworkRetailStoreVPC2A069619D"`는 `lattice-retail-store-vpc`라는 이름의 새로운 VPC를 만들고, 그 안에 퍼블릭 서브넷, 프라이빗 서브넷, 라우트 테이블, Internet Gateway, NAT Gateway 등을 구성합니다. 다만 이 코드 조각들에서 VPC Lattice 서비스 자체가 직접 생성되는 것은 아니며, 이후 VPC Lattice 관련 실습에서 사용할 기반 네트워크를 준비하는 역할에 가깝습니다.

VPC Lattice는 VPC 안팎의 서비스들이 서로 통신할 수 있게 해주는 AWS의 애플리케이션 네트워킹 서비스입니다. 단순히 네트워크를 연결하는 것을 넘어, 서비스 간 연결, 접근 제어, 트래픽 관리, 모니터링을 서비스 단위로 관리할 수 있게 해줍니다.

---
## 4. `"NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3"`

```json
  "NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3": {
   "Type": "AWS::EC2::Subnet",
   "Properties": {
    "AvailabilityZone": {
     "Fn::Sub": [
      "${Region}a",
      {
       "Region": {
        "Ref": "AWS::Region"
       }
      }
     ]
    },
    "CidrBlock": "192.168.0.0/24",
    "MapPublicIpOnLaunch": true,
    "Tags": [
     {
      "Key": "aws-cdk:subnet-name",
      "Value": "Public"
     },
     {
      "Key": "aws-cdk:subnet-type",
      "Value": "Public"
     },
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
```

VPC2의 첫 번째 퍼블릭 서브넷입니다.

---
## 5. `"NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426"`

```json
  "NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  },
```

라우트 테이블을 생성합니다. 

---
## 6. `"NetworkRetailStoreVPC2PublicSubnet1RouteTableAssociation370D0C06"`

```json
"NetworkRetailStoreVPC2PublicSubnet1RouteTableAssociation370D0C06": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426"
    },
    "SubnetId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3"
    }
   }
  },
```

Route Table Association을 생성합니다. 라우트 테이블을 서브넷에 연결하기 위해 `"RouteTableId"`와 `"SubnetId"`를 참조합니다.

---
## 7. `"NetworkRetailStoreVPC2PublicSubnet1DefaultRoute3EAAA9F0"`

```json
  "NetworkRetailStoreVPC2PublicSubnet1DefaultRoute3EAAA9F0": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "NetworkRetailStoreVPC2IGW56CDF441"
    },
    "RouteTableId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1RouteTable8340B426"
    }
   },
   "DependsOn": [
    "NetworkRetailStoreVPC2VPCGW00725957"
   ]
  },
```

라우트 리소스입니다. 라우트 테이블에 하나의 라우팅 규칙을 추가합니다.

---
## 8. `"NetworkRetailStoreVPC2PublicSubnet1EIP7692665A"`

```json
  "NetworkRetailStoreVPC2PublicSubnet1EIP7692665A": {
   "Type": "AWS::EC2::EIP",
   "Properties": {
    "Domain": "vpc",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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

EIP를 생성합니다. 

---
## 9. `"NetworkRetailStoreVPC2PublicSubnet1NATGateway83A6036F"`

```json
  "NetworkRetailStoreVPC2PublicSubnet1NATGateway83A6036F": {
   "Type": "AWS::EC2::NatGateway",
   "Properties": {
    "AllocationId": {
     "Fn::GetAtt": [
      "NetworkRetailStoreVPC2PublicSubnet1EIP7692665A",
      "AllocationId"
     ]
    },
    "SubnetId": {
     "Ref": "NetworkRetailStoreVPC2PublicSubnet1Subnet880F5AC3"
    },
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Network/RetailStoreVPC2/PublicSubnet1"
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
   "DependsOn": [
    "NetworkRetailStoreVPC2PublicSubnet1DefaultRoute3EAAA9F0",
    "NetworkRetailStoreVPC2PublicSubnet1RouteTableAssociation370D0C06"
   ]
  },
```

NAT Gateway를 생성합니다.

---

VPC2의 퍼블릭 서브넷 1과 퍼블릭 서브넷 2의 기본 설정은 거의 동일합니다. 둘 다 VPC2에 속한 퍼블릭 서브넷이며, 인터넷 게이트웨이로 향하는 기본 라우트를 가지고 있습니다. 이 기본 라우트가 참조하는 Internet Gateway와, 이를 VPC에 연결하는 VPC Gateway Attachment 설정도 Fundamentals Lab에서 살펴본 구성과 거의 동일합니다. 차이점은 퍼블릭 서브넷 1에는 NAT Gateway와 이를 위한 Elastic IP가 함께 생성된다는 점입니다. 반면 퍼블릭 서브넷 2에는 별도의 NAT Gateway가 생성되지 않습니다. 이는 워크숍 환경에서 비용과 구성을 단순화하기 위해 하나의 NAT Gateway만 사용하는 구조로 볼 수 있습니다.

---

프라이빗 서브넷의 기본 설정은 Fundamentals Lab의 프라이빗 서브넷 설정과 거의 동일합니다. 다만 VPC2에서는 NAT Gateway를 하나만 생성하고, 여러 프라이빗 서브넷이 이 NAT Gateway를 함께 사용하도록 구성되어 있습니다. 이 차이를 제외하면 서브넷, 라우트 테이블, 기본 라우트의 구성 방식은 앞에서 살펴본 내용과 유사하므로, 이 포스트에서는 별도로 다루지 않겠습니다.

---
## 10. `"NetworkGuardDutyVPCEndpointVPCLattice968F5B1D"`

```json
  "NetworkGuardDutyVPCEndpointVPCLattice968F5B1D": {
   "Type": "AWS::EC2::VPCEndpoint",
   "Properties": {
    "PrivateDnsEnabled": true,
    "SecurityGroupIds": [
     {
      "Fn::GetAtt": [
       "NetworkGuardDutyEndpointSecurityGroupVPCLatticeEA4520D9",
       "GroupId"
      ]
     }
    ],
    "ServiceName": {
     "Fn::Join": [
      "",
      [
       "com.amazonaws.",
       {
        "Ref": "AWS::Region"
       },
       ".guardduty-data"
      ]
     ]
    },
    "SubnetIds": [
     {
      "Ref": "NetworkRetailStoreVPC2PrivateSubnet1SubnetFD8E08D9"
     },
     {
      "Ref": "NetworkRetailStoreVPC2PrivateSubnet2Subnet85F8E521"
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
    ],
    "VpcEndpointType": "Interface",
    "VpcId": {
     "Ref": "NetworkRetailStoreVPC2A069619D"
    }
   }
  }
```

`"NetworkGuardDutyVPCEndpointVPCLattice968F5B1D"`는 VPC2 안에 GuardDuty용 VPC Endpoint를 생성하는 리소스입니다. Amazon GuardDuty는 AWS 계정과 리소스에서 수상한 활동을 탐지해주는 보안 서비스입니다. 

---

`"PrivateDnsEnabled": true` 옵션을 사용하면, Interface Endpoint 생성 시 AWS Route 53 Resolver가 해당 서비스의 기존 퍼블릭 DNS 이름을 Interface Endpoint의 프라이빗 IP로 해석할 수 있게 해줍니다. 즉, 애플리케이션은 기존 AWS 서비스 도메인을 그대로 사용하더라도, 실제 통신은 VPC Endpoint를 통해 프라이빗하게 이루어질 수 있습니다. `"PrivateDnsEnabled": true` 옵션이 동작하려면 VPC 설정에서 `"EnableDnsHostnames": true`와 `"EnableDnsSupport": true`가 활성화되어 있어야 합니다.

간단하게 말하면, Route 53 Resolver가 AWS Cloud 내에서 작동하는 서비스의 기본 DNS 이름을 Interface Endpoint ENI의 프라이빗 IP로 자동 매핑해서, 애플리케이션이 이 프라이빗 IP를 목적지로 하는 요청을 Interface Endpoint의 ENI에 보내는 것입니다. 애플리케이션은 일단 VPC Endpoint의 ENI까지만 찾아가고, 그 뒤는 AWS가 PrivateLink를 통해 실제 AWS 서비스까지 이어주는 구조입니다.

만약 Route 53 Resolver가 이 매핑을 하지 않는다면, 애플리케이션이 기존 AWS 서비스 도메인을 사용하더라도 Interface Endpoint의 프라이빗 IP로 연결되지 않습니다. 따라서 요청은 VPC Endpoint의 ENI가 아니라 일반 AWS 서비스 엔드포인트로 향하게 됩니다. 즉, `0.0.0.0/0 → NAT Gateway` 라우트 규칙이 있으면 요청이 NAT Gateway를 통해 전달될 수 있고, 그런 외부 경로가 없다면 요청은 실패하게 됩니다.

---

`"ServiceName"`은 이 VPC Endpoint가 연결할 AWS 서비스를 지정하는 설정입니다. 여기서는 `Fn::Join`을 사용해 `com.amazonaws.<현재 리전>.guardduty-data` 형식의 서비스 이름을 만들고 있습니다. 예를 들어 서울 리전에서는 `com.amazonaws.ap-northeast-2.guardduty-data`가 되며, 이는 현재 리전의 GuardDuty Data 서비스에 연결되는 VPC Endpoint를 생성한다는 의미입니다.

---

`"SubnetIds"`는 Interface VPC Endpoint가 생성될 서브넷을 지정하는 설정입니다. 여기서는 VPC2의 프라이빗 서브넷 1과 프라이빗 서브넷 2를 지정하고 있으므로, 각 서브넷에 Endpoint ENI가 생성됩니다. 이를 통해 해당 프라이빗 서브넷의 리소스들이 GuardDuty Data 서비스에 프라이빗 경로로 접근할 수 있습니다.


