---
title: "03. 프라이빗 서브넷 환경 (서브넷, 라우팅)"
weight: 3
---
> **작성일:** 2026-04-18 | **수정일:** 2026-04-20
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

---

프라이빗 서브넷을 생성합니다. 

---

```json
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
```

이 프라이빗 서브넷은 `${Region}a` AZ에 배치됩니다. 앞서 본 첫 번째 퍼블릭 서브넷도 같은 `${Region}a` AZ에 있었으므로, 두 서브넷은 같은 AZ에 위치합니다.

---

`"CidrBlock": "10.0.3.0/24"`는 이 프라이빗 서브넷이 `10.0.3.0/24` 대역의 IP 주소 범위를 사용한다는 뜻입니다. 주소 범위로 보면 `10.0.3.0`부터 `10.0.3.255`까지에 해당합니다. 앞의 퍼블릭 서브넷들이 `10.0.1.0/24`, `10.0.2.0/24`를 사용하므로, 이 프라이빗 서브넷은 그 다음 범위인 `10.0.3.0/24`를 사용합니다. 다만 AWS는 서브넷마다 일부 IP 주소를 예약하므로, 이 범위의 모든 IP를 리소스에 직접 사용할 수 있는 것은 아닙니다.

---

`"MapPublicIpOnLaunch": false` 옵션은 이 프라이빗 서브넷에서 시작되는 인스턴스에 Public IP를 자동으로 할당하지 않도록 합니다. 프라이빗 서브넷의 인스턴스는 인터넷에서 직접 접근하는 대상이 아니기 때문에, 보통 이 값을 `false`로 설정합니다.

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

라우트 테이블을 생성합니다.

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

라우트 테이블 어소시에이션(Route Table Association)을 생성합니다. 이 리소스는 서브넷과 라우트 테이블을 연결합니다.

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

프라이빗 서브넷의 라우트 테이블에 기본 라우트(`0.0.0.0/0`)를 추가합니다. 이를 통해 프라이빗 서브넷의 인스턴스가 인터넷으로 나가는 트래픽을 NAT 게이트웨이를 통해 전달할 수 있습니다.

---

프라이빗 서브넷의 리소스는 일반적으로 인터넷에서 직접 접근하는 대상이 아니며, 보통 Public IP도 자동으로 할당되지 않습니다. 따라서 인터넷으로 나가는 요청은 NAT Gateway를 통해 전달됩니다. 이때 NAT Gateway는 요청 패킷의 출발지 사설 IP와 포트 번호를 자신의 Public IP와 포트 번호로 변환한 뒤 인터넷으로 전달합니다. 이후 NAT Gateway가 응답 패킷을 받으면, 이 매핑 정보를 바탕으로 해당 패킷이 원래 리소스로 돌아갈 수 있도록 전달합니다.

```
원래 요청
10.0.3.10:52341 -> 8.8.8.8:443

NAT Gateway 변환 후
3.3.3.3:40001 -> 8.8.8.8:443

응답 수신
8.8.8.8:443 -> 3.3.3.3:40001

NAT Gateway 재변환 후
8.8.8.8:443 -> 10.0.3.10:52341
```

예를 들어 `10.0.3.10:52341`에서 보낸 요청은 NAT Gateway를 거치면서 `3.3.3.3:40001`로 변환되어 외부로 나가고, 응답이 돌아오면 다시 `10.0.3.10:52341`로 변환되어 원래 인스턴스에 전달됩니다.

---

나머지 코드도 다른 AZ의 프라이빗 서브넷에 대한 동일한 구성입니다. 설정이 거의 같으므로 추가 분석은 생략하겠습니다.
