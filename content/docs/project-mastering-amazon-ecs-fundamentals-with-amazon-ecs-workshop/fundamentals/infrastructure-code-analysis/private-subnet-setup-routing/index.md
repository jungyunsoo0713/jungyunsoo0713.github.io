---
title: "03. 프라이빗 서브넷 환경 (서브넷, 라우팅)"
weight: 3
---
> **작성일:** 2026-05-02 | **수정일:** 2026-05-02
```json
{
  "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4": {
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
    "CidrBlock": "10.0.3.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet1RouteTableDF70C60F": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet1RouteTableDF70C60F"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "NatGatewayId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1NATGatewayF5759A33"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet1RouteTableDF70C60F"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638": {
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
    "CidrBlock": "10.0.4.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet2"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
	  },
  "BasicRetailStoreVPCPrivateSubnet2RouteTable88B6A240": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet2"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet2RouteTableAssociationA0687748": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet2RouteTable88B6A240"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet2Subnet5E255638"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet2DefaultRoute6CB95A04": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "NatGatewayId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet2NATGateway304D4FCE"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet2RouteTable88B6A240"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173": {
   "Type": "AWS::EC2::Subnet",
   "Properties": {
    "AvailabilityZone": {
     "Fn::Sub": [
      "${Region}c",
      {
       "Region": {
        "Ref": "AWS::Region"
       }
      }
     ]
    },
    "CidrBlock": "10.0.5.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet3"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet3RouteTableE3807248": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet3"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet3RouteTableAssociationF2AF7FA5": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet3RouteTableE3807248"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet3Subnet01DB8173"
    }
   }
  },
  "BasicRetailStoreVPCPrivateSubnet3DefaultRoute11F29836": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "NatGatewayId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet3NATGateway700166B7"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet3RouteTableE3807248"
    }
   }
  }
}
```

---

## 1. `"BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"`

```json
  "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4": {
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
    "CidrBlock": "10.0.3.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

첫 번째 프라이빗 서브넷을 생성하는 리소스입니다.

---

`"AvailabilityZone"`을 보면 이 프라이빗 서브넷이 첫 번째 퍼블릭 서브넷과 같은 가용 영역(AZ)에 위치해 있음을 알 수 있습니다.

---

`"CidrBlock": "10.0.3.0/24"`의 대역은 `10.0.3.0`부터 `10.0.3.255`까지입니다. 앞의 3개의 퍼블릭 서브넷의 대역이 각각 `10.0.0.0/24`, `10.0.1.0/24`, `10.0.2.0/24`이기 때문에 이 대역 바로 뒤의 대역을 쓴 것임을 알 수 있습니다.

---

`"MapPublicIpOnLaunch": false`는 이 프라이빗 서브넷에 위치한 인스턴스에게 자동으로 퍼블릭 IPv4 주소를 할당하는 것을 비활성화합니다. 이는 프라이빗 서브넷의 인스턴스는 기본적으로 인터넷에서 접근되지 않기 때문입니다.

---

## 2. `"BasicRetailStoreVPCPrivateSubnet1RouteTableDF70C60F"`

```json
  "BasicRetailStoreVPCPrivateSubnet1RouteTableDF70C60F": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PrivateSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

첫 번째 프라이빗 서브넷에 위치할 라우트 테이블을 생성하는 리소스입니다.

---

## 3. `"BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100"`

```json
  "BasicRetailStoreVPCPrivateSubnet1RouteTableAssociationCBCC2100": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet1RouteTableDF70C60F"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet1Subnet6D036CC4"
    }
   }
  },
```

라우트 테이블 어소시에이션(Route Table Association)을 생성하는 리소스입니다.

---

## 4. `"BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2"`

```json
  "BasicRetailStoreVPCPrivateSubnet1DefaultRoute701604F2": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "NatGatewayId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1NATGatewayF5759A33"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPrivateSubnet1RouteTableDF70C60F"
    }
   }
  },
```

라우트를 생성하는 리소스입니다.

퍼블릭 서브넷에 연결된 라우트 테이블의 기본 라우트 규칙과 다르게, 이 라우트 규칙에 해당하는 트래픽은 IGW가 아닌 NAT Gateway로 전달되도록 명시됩니다.

---

나머지 리소스들은 서로 다른 AZ에 속해 있지만 내부 설정이 동일한 프라이빗 서브넷 관련 리소스들이므로, 이에 대한 코드 분석은 생략하겠습니다.
