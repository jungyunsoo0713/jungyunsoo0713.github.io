---
title: "01. VPC 및 네트워크 인프라 (VPC, Subnets, Gateways, Endpoints, DNS"
weight: 1
---
> **작성일:** 2026-05-04 | **수정일:** 2026-05-04
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

이 리소스는 프라이빗 DNS 네임스페이스를 생성하는 리소스입니다. 프라이빗 DNS Namespace는 `orders.retailstore.local` 같은 프라이빗 DNS 이름과 프라이빗 IP 주소의 연결 정보(DNS 레코드 정보)가 저장되고 관리되는 DNS 이름 공간입니다. AWS Route 53 Resolver는 이 프라이빗 DNS Namespace와 연결된 DNS 레코드 정보를 사용하여 DNS 해석 기능을 제공합니다.

DNS Namespace에는 이런 구조의 DNS 레코드 정보가 저장되고 관리됩니다.

```
orders.retailstore.local  →  10.0.1.25
orders.retailstore.local  →  10.0.2.41
checkout.retailstore.local → 10.0.3.18
```

Orders 서비스의 태스크가 2개 있으면 `orders.retailstore.local` 하나가 여러 프라이빗 IP로 해석될 수 있습니다.

---

`"Name"`는 생성할 프라이빗 DNS Namespace의 이름을 지정하는 필드입니다. `"retailstore.local"`가 DNS Namespace의 이름입니다.

---

`"Vpc"`는 이 Private DNS Namespce가 위치할 VPC를 지정하는 필드입니다.

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

타깃 그룹을 생성하는 리소스입니다. UI 서비스로 HTTPS 트래픽을 전달하기 위한 ALB 타깃 그룹입니다.

---

`"HealthCheckPort": "8080"`은 타깃 그룹에 등록된 타깃에게 헬스 체크를 보낼 때 사용하는 포트입니다. 예시로 `https://10.0.1.25:8080/actuator/health` 와 같은 엔드포인트를 사용하여 헬스 체크 요청을 보냅니다. 

`"Port": 8080`는 타깃 그룹에 등록된 타깃이 요청을 받는 포트입니다. 예시 엔드포인트는 `https://10.0.1.25:8080`와 같습니다.

만약 `"HealthCheckPort"`가 지정되어 있지 않다면, 타깃 그룹의 `"Port"` 필드의 값을 사용합니다.

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

VPC를 생성하는 리소스입니다. VPC Lattice 기능을 사용하기 위한 VPC입니다.

---

`"CidrBlock": "192.168.0.0/16"`은 프라이빗 IP 주소 대역 중 하나인 `192.168.0.0/16` 대역을 사용합니다. 프라이빗 IP 주소 대역은 `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`입니다.

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

첫 번째 퍼블릭 서브넷을 생성하는 리소스입니다.

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

첫 번째 퍼블릭 서브넷에 위치할 라우트 테이블을 생성하는 리소스입니다.

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

라우트 테이블 어소시에이션(Route Table Association)을 생성하는 리소스입니다.

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

라우트를 생성하는 리소스입니다

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

EIP(Elastic IP)를 생성하는 리소스입니다.

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

퍼블릭 서브넷에 위치할 NAT Gateway를 생성하는 리소스입니다.

---

나머지 리소스들은 서로 다른 AZ에 속해 있지만 내부 설정이 동일한 퍼블릭 서브넷 관련 리소스들이므로, 이에 대한 코드 분석은 생략하겠습니다.
