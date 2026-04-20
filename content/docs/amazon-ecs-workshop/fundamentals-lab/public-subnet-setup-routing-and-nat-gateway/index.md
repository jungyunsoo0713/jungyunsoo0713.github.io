---
title: "02. 퍼블릭 서브넷 환경 (서브넷, 라우팅, NAT 게이트웨이)"
weight: 2
---
> **작성일:** 2026-04-17 | **수정일:** 2026-04-20
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

---

첫 번째 퍼블릭 서브넷을 생성합니다.

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

고가용성(High Availability)을 위해 여러 서브넷을 서로 다른 AZ에 배치합니다. `"Fn::Sub"`는 문자열 안의 변수 값을 실제 값으로 바꿔 넣는 CloudFormation 함수입니다. 위 코드는 `"AWS::Region"` 값을 가져온 뒤 뒤에 `"a"`를 붙여, 예를 들어 `ap-northeast-2a`와 같은 AZ 이름을 만듭니다.

---

`"CidrBlock": "10.0.0.0/24"`를 보면, 이 IP 주소 범위가 VPC의 IP 주소 범위의 일부라는 것을 알 수 있습니다. VPC의 IP 주소 범위는 `"10.0.0.0/16"`이며, 이는 `10.0.0.0`부터 `10.0.255.255`까지의 IP 주소를 의미합니다. `"10.0.0.0/24"`는 앞의 24비트가 네트워크 주소를 식별하는 데 사용되므로, 이 범위는 `10.0.0.0`부터 `10.0.0.255`까지의 IP 주소를 의미합니다. 따라서 이 퍼블릭 서브넷은 VPC의 IP 주소 범위의 1/256 크기입니다.

---

`"MapPublicIpOnLaunch": true`는 이 서브넷에서 시작되는 인스턴스에 Public IPv4 주소를 자동으로 할당하는 옵션입니다. 퍼블릭 서브넷에는 일반적으로 인터넷과 직접 통신해야 하는 인스턴스를 배치하므로, Public IP를 자동으로 할당하도록 설정하는 경우가 많습니다.

참고로 여기서 말하는 인스턴스는 보통 EC2 인스턴스를 뜻합니다. EC2 인스턴스는 AWS 리소스의 한 종류이며, 서브넷, IGW, NAT Gateway도 각각 AWS 리소스입니다.

---

```json
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
```

서브넷은 반드시 어떤 VPC 안에 생성되어야 하므로, `"VpcId"`에서 해당 VPC를 `"Ref"`로 참조합니다.

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

---

라우트 테이블을 생성합니다. 라우트 테이블은 서브넷의 리소스가 트래픽을 보낼 때, 목적지에 따라 그 트래픽을 어디로 전달할지 결정하는 규칙들의 집합입니다.

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

---

```json
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9"
    },
    "SubnetId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
    }
```

Route Table Association은 생성된 라우트 테이블을 서브넷에 연결하는 리소스입니다. 따라서 `"RouteTableId"`와 `"SubnetId"`를 참조(`"Ref"`)합니다.

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

라우트 리소스입니다. 라우트 테이블에 하나의 라우팅 규칙을 추가합니다.

---

```json
    "DestinationCidrBlock": "0.0.0.0/0",
    "GatewayId": {
     "Ref": "BasicRetailStoreVPCIGW448D34CF"
    },
```

`"DestinationCidrBlock": "0.0.0.0/0"`은 모든 IPv4 대상에 대한 트래픽을 의미합니다. 이 라우트는 `"GatewayId"`가 `"BasicRetailStoreVPCIGW448D34CF"`를 참조하고 있으므로, 해당 트래픽을 Internet Gateway로 전달합니다.

---

```json
    "RouteTableId": {
     "Ref": "BasicRetailStoreVPCPublicSubnet1RouteTable28FE76F9"
    },
```

이 라우팅 규칙이 추가될 라우트 테이블을 참조합니다.

---

```json
   "DependsOn": [
    "BasicRetailStoreVPCVPCGW9E2A7641"
   ]
```

`"BasicRetailStoreVPCVPCGW9E2A7641"`는 앞서 생성한 VPC Gateway Attachment입니다.

`"DependsOn"`은 이 리소스(라우트)가 생성되기 전에 해당 리소스(VPC Gateway Attachment)가 먼저 생성되어야 함을 의미합니다. VPC Gateway Attachment가 먼저 생성되었다는 것은 IGW가 VPC에 연결되었다는 뜻이며, 그래야 라우트에 정의된 규칙(모든 IPv4 대상에 대한 트래픽을 IGW로 전달하는 규칙)을 유효하게 만들 수 있습니다.

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

EIP를 생성합니다. 

---

EIP는 리소스에 떼었다 붙였다 할 수 있는 고정 Public IPv4 주소입니다. Public IP는 ISP가 할당 가능한 IP를 할당하는 것이기 때문에, Private IP와 달리 일반적인 Public IP는 리소스가 다시 생성되면 다른 값으로 할당될 수 있습니다. 반면 EIP는 고정된 주소이므로, 같은 EIP를 다시 연결하면 그 값이 바뀌지 않습니다. 예를 들어 NAT Gateway를 다시 생성하더라도, 같은 EIP를 다시 연결하면 외부에 보이는 IP 주소를 그대로 유지할 수 있습니다. EIP는 연결 대상 리소스와 별개의 AWS 리소스이므로, 연결된 리소스가 삭제되어도 EIP 자체는 계정에 남아 다시 다른 리소스에 연결할 수 있습니다.

이 EIP는 서브넷 자체에 연결되는 것이 아니라, 나중에 이 퍼블릭 서브넷에 생성될 NAT Gateway가 사용할 수 있도록 미리 생성한 것으로 보입니다.

---

`"Domain": "vpc"`는 이 EIP를 VPC 환경에서 사용할 수 있도록 생성한다는 뜻입니다. 이렇게 생성된 EIP는 NAT Gateway 같은 VPC 리소스에 연결할 수 있습니다. 이때 EIP에는 `"AllocationId"`가 부여되며, NAT Gateway는 이 값을 사용해 EIP를 연결합니다.

---

## 6. `"BasicRetailStoreVPCPublicSubnet1NATGatewayF5759A33"`

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

NAT Gateway를 생성합니다.

---

```json
    "AllocationId": {
     "Fn::GetAtt": [
      "BasicRetailStoreVPCPublicSubnet1EIP375531A2",
      "AllocationId"
     ]
```

`"AllocationId"`는 NAT Gateway에 연결할 EIP의 식별자입니다. NAT Gateway에 EIP를 연결할 때는 VPC에 VPC Gateway Attachment로 IGW를 연결하거나 Route Table에 Route를 추가할 때처럼 `"Ref"`로 참조하는 것이 아니라, EIP의 `"AllocationId"`를 참조해야 합니다. 이는 `"AWS::EC2::EIP"`에서 `"Ref"`가 EIP의 Public IPv4 주소를 반환하는 반면, NAT Gateway의 `"AllocationId"` 속성은 EIP의 allocation ID를 요구하기 때문입니다.

`"Fn::GetAtt"`는 CloudFormation의 함수로, 특정 리소스의 특정 속성값(attribute)을 가져옵니다. 여기서는 앞서 생성한 EIP의 `"AllocationId"`값을 가져옵니다.

---

```json
   "DependsOn": [
    "BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5",
    "BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702"
   ]
```

이 NAT Gateway가 라우트와 라우트 테이블 어소시에이션 이후에 생성되도록 순서를 명시합니다. 즉, 이 템플릿에서는 해당 서브넷이 인터넷 게이트웨이를 사용하는 퍼블릭 서브넷으로 먼저 구성된 뒤 NAT Gateway를 생성하도록 한 것입니다.

---

나머지 코드들은 서로 다른 AZ에 속해 있지만 내부 설정이 동일한 퍼블릭 서브넷들이므로, 이에 대한 코드 분석은 생략하겠습니다.
