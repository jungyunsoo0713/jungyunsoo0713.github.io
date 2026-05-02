---
title: "02. 퍼블릭 서브넷 환경 (서브넷, 라우팅, NAT 게이트웨이)"
weight: 2
---
> **작성일:** 2026-05-02 | **수정일:** 2026-05-02
```json
{
  "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8": {
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
    "CidrBlock": "10.0.0.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
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
  "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
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
  "BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
    }
   }
  },
  "BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "BasicRetailStoreVPCIGW448D34CF"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9"
    }
   },
   "DependsOn": [
    "BasicRetailStoreVPCVPCGW9E2A7641"
   ]
  },
  "BasicRetailStoreVPCPublicSubnet1EIP375531A2": {
   "Type": "AWS::EC2::EIP",
   "Properties": {
    "Domain": "vpc",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   }
  },
  "BasicRetailStoreVPCPublicSubnet1NATGatewayF5759A33": {
   "Type": "AWS::EC2::NatGateway",
   "Properties": {
    "AllocationId": {
     "Fn::GetAtt": [
      "BasicRetailStoreVPCPublicSubnet1EIP375531A2",
      "AllocationId"
     ]
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
    },
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   },
   "DependsOn": [
    "BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5",
    "BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702"
   ]
  },
  "BasicRetailStoreVPCPublicSubnet2Subnet3227177D": {
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
    "CidrBlock": "10.0.1.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet2"
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
  "BasicRetailStoreVPCPublicSubnet2RouteTableD768142F": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet2"
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
  "BasicRetailStoreVPCPublicSubnet2RouteTableAssociation661C9073": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet2RouteTableD768142F"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet2Subnet3227177D"
    }
   }
  },
  "BasicRetailStoreVPCPublicSubnet2DefaultRoute687061C9": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "BasicRetailStoreVPCIGW448D34CF"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet2RouteTableD768142F"
    }
   },
   "DependsOn": [
    "BasicRetailStoreVPCVPCGW9E2A7641"
   ]
  },
  "BasicRetailStoreVPCPublicSubnet2EIP34315004": {
   "Type": "AWS::EC2::EIP",
   "Properties": {
    "Domain": "vpc",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet2"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   }
  },
  "BasicRetailStoreVPCPublicSubnet2NATGateway304D4FCE": {
   "Type": "AWS::EC2::NatGateway",
   "Properties": {
    "AllocationId": {
     "Fn::GetAtt": [
      "BasicRetailStoreVPCPublicSubnet2EIP34315004",
      "AllocationId"
     ]
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet2Subnet3227177D"
    },
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet2"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   },
   "DependsOn": [
    "BasicRetailStoreVPCPublicSubnet2DefaultRoute687061C9",
    "BasicRetailStoreVPCPublicSubnet2RouteTableAssociation661C9073"
   ]
  },
  "BasicRetailStoreVPCPublicSubnet3Subnet3BE6B890": {
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
    "CidrBlock": "10.0.2.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet3"
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
  "BasicRetailStoreVPCPublicSubnet3RouteTableF6D8C2E0": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet3"
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
  "BasicRetailStoreVPCPublicSubnet3RouteTableAssociationDA4CEA25": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet3RouteTableF6D8C2E0"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet3Subnet3BE6B890"
    }
   }
  },
  "BasicRetailStoreVPCPublicSubnet3DefaultRoute17E6A8A5": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "BasicRetailStoreVPCIGW448D34CF"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet3RouteTableF6D8C2E0"
    }
   },
   "DependsOn": [
    "BasicRetailStoreVPCVPCGW9E2A7641"
   ]
  },
  "BasicRetailStoreVPCPublicSubnet3EIP49616D92": {
   "Type": "AWS::EC2::EIP",
   "Properties": {
    "Domain": "vpc",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet3"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   }
  },
  "BasicRetailStoreVPCPublicSubnet3NATGateway700166B7": {
   "Type": "AWS::EC2::NatGateway",
   "Properties": {
    "AllocationId": {
     "Fn::GetAtt": [
      "BasicRetailStoreVPCPublicSubnet3EIP49616D92",
      "AllocationId"
     ]
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet3Subnet3BE6B890"
    },
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet3"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   },
   "DependsOn": [
    "BasicRetailStoreVPCPublicSubnet3DefaultRoute17E6A8A5",
    "BasicRetailStoreVPCPublicSubnet3RouteTableAssociationDA4CEA25"
   ]
  }
}
```

---

## 1. `"BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"`

```json
  "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8": {
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
    "CidrBlock": "10.0.0.0/24",
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
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
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

첫 번째 퍼블릭 서브넷을 생성하는 리소스입니다.

---

`"AvailabilityZone"`는 이 퍼블릭 서브넷이 위치한 가용 영역(AZ)을 의미합니다. `"Fn::Sub"`는 CloudFormation의 함수로, `"${Region}a"`에서 `"${Region}"` 부분을 `Region` 값으로 대체합니다. `"Region"` 필드는 `"AWS::Region"` 값을 참조합니다. `"AWS::Region"`은 현재 스택이 배포되는 리전을 의미합니다. 이 `Region` 값이 예를 들어 `ap-northeast-2`라면, 최종 결과는 `ap-northeast-2a`가 됩니다.

---

`"CidrBlock": "10.0.0.0/24"`는 이 퍼블릭 서브넷이 가지는 주소 대역을 명시합니다.  
주소 대역은 `10.0.0.0`부터 `10.0.0.255`까지로, 총 256개의 IP 주소를 가집니다. 하지만 이전에 언급했듯이 AWS는 서브넷의 일부 주소를 예약하기 때문에 실제 리소스에 할당될 수 있는 주소의 개수는 256개보다 더 적습니다.

---

`"MapPublicIpOnLaunch": true`는 이 서브넷에서 생성되는 인스턴스에 자동으로 퍼블릭 IPv4 주소를 부여하는 옵션입니다.

---

`"VpcId"` 필드에서 `"Ref"`로 VPC의 Logical ID를 참조합니다. 이는 실제 생성된 VPC의 VPC ID를 반환하여, 이 퍼블릭 서브넷이 위치할 VPC를 명시합니다.

---

## 2. `"BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9"`

```json
  "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9": {
   "Type": "AWS::EC2::RouteTable",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
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

첫 번째 퍼블릭 서브넷에 위치할 라우트 테이블을 생성하는 리소스입니다.

---

`"VpcId"` 필드에서 `"Ref"`로 VPC의 Logical ID를 참조합니다. 이는 실제 생성된 VPC의 VPC ID를 반환하여, 이 라우트 테이블이 위치할 VPC를 명시합니다.

---

## 3. `"BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702"`

```json
  "BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702": {
   "Type": "AWS::EC2::SubnetRouteTableAssociation",
   "Properties": {
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
    }
   }
  },
```

라우트 테이블 어소시에이션(Route Table Association)을 생성하는 리소스입니다. 이 리소스는 라우트 테이블과 서브넷을 연결합니다.

---

`"RouteTableId"`와 `"SubnetId"`는 `"Ref"`로 각 리소스의 Logical ID를 참조합니다. 이를 통해 실제 생성된 Route Table ID와 Subnet ID를 가져와, 해당 서브넷을 라우트 테이블에 연결합니다.

---

## 4. `"BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5"`

```json
  "BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5": {
   "Type": "AWS::EC2::Route",
   "Properties": {
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "BasicRetailStoreVPCIGW448D34CF"
    },
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9"
    }
   },
   "DependsOn": [
    "BasicRetailStoreVPCVPCGW9E2A7641"
   ]
  },
```

라우트를 생성하는 리소스입니다. 라우트는 라우트 테이블에 추가되는 규칙입니다. 이 라우트는 기본 라우트로, 다른 라우트 규칙에 해당하지 않는 트래픽이 전달되는 경로를 정합니다.

---

`"DestinationCidrBlock"`은 트래픽의 목적지 IP 주소 범위를 말합니다. `"0.0.0.0/0"`은 모든 IPv4 목적지 IP 주소를 의미합니다. 즉, `"DestinationCidrBlock": "0.0.0.0/0"`은 다른 라우트 규칙에 해당하지 않는 모든 IPv4 트래픽을 지칭합니다.

---

`"RouteTableId"` 필드에서 `"Ref"`로 라우트 테이블의 Logical ID를 참조합니다. 이는 실제 생성된 라우트 테이블의 라우트 테이블 ID를 반환하여, 이 라우트가 연결될 라우트 테이블을 명시합니다.

---

`"DependsOn"`은 이 리소스가 의존하는 리소스를 가리킵니다. 따라서 `"DependsOn"`에 지정된 리소스가 먼저 생성된 뒤에 이 리소스가 생성됩니다. 지정된 리소스인 VPC가 먼저 생성되어야 이 리소스가 제대로 생성될 수 있음을 알 수 있습니다.

---

## 5. `"BasicRetailStoreVPCPublicSubnet1EIP375531A2"`

```json
  "BasicRetailStoreVPCPublicSubnet1EIP375531A2": {
   "Type": "AWS::EC2::EIP",
   "Properties": {
    "Domain": "vpc",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   }
  },
```

EIP(Elastic IP)를 생성하는 리소스입니다. EIP는 리소스에 붙였다 뗐다 할 수 있는 퍼블릭 IP 주소입니다. 일반적으로 VPC 내부의 인스턴스에 자동으로 부여된 퍼블릭 IP 주소는, 해당 인스턴스가 중지되거나 재생성되면 그 퍼블릭 IP 주소값이 바뀔 수 있습니다. EIP는 해당 인스턴스가 중지되거나 재생성되어도, 떼어진 EIP를 다시 붙이면 되기 때문에 동일한 IP 주소값을 계속 사용할 수 있습니다.

---

`"Domain": "vpc"`는 이 EIP가 VPC에서 사용될 것임을 명시합니다. 현재 EIP는 사실상 VPC 내부에서 사용됩니다. 다만 레거시 시스템에서는 `"vpc"` 값 대신 `"standard"` 값도 존재했기 때문에, `"vpc"` 값을 주어 구별할 수 있습니다. 참고로 이 옵션을 사용하지 않더라도 CloudFormation에서는 기본값인 `"vpc"`를 사용합니다.

---

## 5. `"BasicRetailStoreVPCPublicSubnet1NATGatewayF5759A33"`

```json
  "BasicRetailStoreVPCPublicSubnet1NATGatewayF5759A33": {
   "Type": "AWS::EC2::NatGateway",
   "Properties": {
    "AllocationId": {
     "Fn::GetAtt": [
      "BasicRetailStoreVPCPublicSubnet1EIP375531A2",
      "AllocationId"
     ]
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
    },
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "EcsImmersionDay/Basic/RetailStoreVPC/PublicSubnet1"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   },
   "DependsOn": [
    "BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5",
    "BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702"
   ]
  },
```

퍼블릭 서브넷에 위치할 NAT Gateway를 생성하는 리소스입니다.

---

`"AllocationId"`는 EIP를 식별하는 ID입니다. NAT Gateway에 EIP를 붙일 때는, NAT Gateway에 EIP의 `"AllocationId"`를 할당함으로써 연결됩니다.

`"Fn::GetAtt"`은 CloudFormation의 함수로, 리소스의 Logical ID를 통해 특정 속성을 가져옵니다. 여기서는 EIP의 `"AllocationId"`를 가져옵니다.

---

`"SubnetId"` 필드에서 `"Ref"`로 퍼블릭 서브넷의 Logical ID를 참조합니다. 이는 실제 생성된 서브넷의 서브넷 ID를 반환하여, 이 NAT Gateway가 연결될 서브넷을 명시합니다.

---

`"DependsOn"`은 이 리소스가 의존하는 리소스를 가리킵니다. 따라서 `"DependsOn"`에 지정된 리소스가 먼저 생성된 뒤에 이 리소스가 생성됩니다. 지정된 리소스인 라우트와 라우트 테이블 어소시에이션이 먼저 생성되어야 이 리소스가 제대로 생성될 수 있음을 알 수 있습니다.

---

나머지 리소스들은 서로 다른 AZ에 속해 있지만 내부 설정이 동일한 퍼블릭 서브넷 관련 리소스들이므로, 이에 대한 코드 분석은 생략하겠습니다.
