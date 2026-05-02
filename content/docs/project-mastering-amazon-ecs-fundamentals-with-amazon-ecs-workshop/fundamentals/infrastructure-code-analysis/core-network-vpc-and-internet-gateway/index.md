---
title: "01. 코어 네트워크 (VPC & 인터넷 게이트웨이)"
weight: 1
---
> **작성일:** 2026-05-02 | **수정일:** 2026-05-02
```json
{
  "BasicRetailStoreVPC0FA21CD5": {
   "Type": "AWS::EC2::VPC",
   "Properties": {
    "CidrBlock": "10.0.0.0/16",
    "EnableDnsHostnames": true,
    "EnableDnsSupport": true,
    "InstanceTenancy": "default",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "retail-store-vpc"
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
  "BasicRetailStoreVPCIGW448D34CF": {
   "Type": "AWS::EC2::InternetGateway",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "retail-store-vpc"
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
  "BasicRetailStoreVPCVPCGW9E2A7641": {
   "Type": "AWS::EC2::VPCGatewayAttachment",
   "Properties": {
    "InternetGatewayId": {
     "Ref": "BasicRetailStoreVPCIGW448D34CF"
    },
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  }
}
```

---

## 1. `"BasicRetailStoreVPC0FA21CD5"`

```json
  "BasicRetailStoreVPC0FA21CD5": {
   "Type": "AWS::EC2::VPC",
   "Properties": {
    "CidrBlock": "10.0.0.0/16",
    "EnableDnsHostnames": true,
    "EnableDnsSupport": true,
    "InstanceTenancy": "default",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "retail-store-vpc"
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

VPC를 생성하는 리소스입니다. 

---

`"BasicRetailStoreVPC0FA21CD5"`는 이 CloudFormation 템플릿에서 사용되는 리소스의 Logical ID입니다. Logical ID란 템플릿 내부에서 리소스를 식별하기 위한 식별자입니다. 이 템플릿 내부에서만 통용되는 식별자이기 때문에, 이 리소스가 AWS에 배포된다고 해서 Logical ID가 실제 리소스 이름이 되거나 이름에 포함되는 것은 아닙니다.

`"BasicRetailStoreVPC0FA21CD5"`에서 `0FA21CD5`는 AWS CDK가 이 CloudFormation 템플릿을 만들 때 Logical ID에 포함시킨 임의의 해시값입니다. 이 해시값은 리소스의 Logical ID가 중복되는 것을 막기 위해 사용됩니다.

---

`"Type"`은 이 리소스의 타입을 명시합니다.

`"AWS::EC2::VPC"`와 같은 구조를 리소스 타입 식별자라고 부릅니다. 여기서 `AWS`는 리소스를 제공하는 제공자를, `EC2`는 서비스를, `VPC`는 리소스 타입을 의미합니다. `::` 기호는 계층/네임 스페이스를 나누는 구분자입니다.

따라서, `"Type": "AWS::EC2::VPC"`는 이 리소스의 타입이 VPC 리소스 타입이라는 것을 알려줍니다.

여기서 VPC가 왜 EC2 서비스에 속해 있는지 의문을 가질 수 있습니다. VPC 초창기에는 VPC가 EC2 인스턴스에서 사용할 수 있는 가상 네트워크 기능에 가까웠기 때문에, EC2 네임스페이스 안에 들어가 있는 것이 현재까지 유지되는 것으로 볼 수 있습니다.

---

`"Properties"`는 해당 리소스의 속성값들을 모아놓은 필드입니다.

---

`"CidrBlock"`는 이 리소스에 할당할 네트워크 대역을 명시합니다. 즉, VPC에 할당될 네트워크 주소 범위를 말합니다.

`"10.0.0.0/16"`는 이 VPC의 네트워크 대역 값입니다. 여기서 `/16`은 앞의 16비트가 네트워크 부분이라는 뜻입니다. IPv4 주소는 8비트씩 4개가 모여 총 32비트로 이루어져 있습니다. 따라서 `10.0.0.0/16`에서는 앞의 16비트, 즉 `10.0` 부분이 네트워크 주소를 의미하고, 나머지 16비트는 해당 네트워크 안에서 사용할 수 있는 호스트 주소 부분을 의미합니다. 따라서 `10.0.0.0/16`의 전체 주소 범위는 `10.0.0.0`에서 `10.0.255.255`까지이며, 총 2의 16승인 65,536개의 IP 주소를 가집니다.

참고로 AWS는 각 서브넷마다 일부 IP 주소를 예약해서 사용합니다. 따라서 저 주소 개수보다 더 적은 수를 리소스에 할당하는 데 사용할 수 있습니다.

---

`"EnableDnsHostnames": true`는 퍼블릭 IPv4 주소를 가진 인스턴스가 DNS 호스트 이름을 가질 수 있게 허용하는 옵션입니다. 

VPC 내부에 배치되는 인스턴스는 인터넷으로부터 요청을 받기 위한 이유 등으로 퍼블릭 IPv4 주소를 할당받을 수 있습니다. 이 인스턴스가 DNS 호스트 이름을 가진다면, 인터넷에서는 이 인스턴스로 요청하기 위해 이 인스턴스가 가진 퍼블릭 IPv4 주소에 매핑되는 DNS 호스트 이름으로 요청할 수 있습니다. 

인스턴스의 퍼블릭 IPv4 주소에 매핑되는 DNS 호스트 이름은 AWS가 직접 할당합니다. AWS Route 53 Resolver가 이 DNS 호스트 이름을 퍼블릭 IPv4 주소로 해석하는 기능을 담당합니다. 덕분에 인스턴스에 요청하는 주체는 인스턴스의 DNS 호스트 이름만 알아도 요청을 보낼 수 있습니다.

---

`"EnableDnsSupport": true`는 VPC 내에서 DNS 해석 기능을 활성화합니다. 이 리소스에서는 `"EnableDnsHostnames": true`와 함께 사용되기 때문에, AWS Route 53 Resolver를 통해 DNS 호스트 이름을 퍼블릭 IPv4 주소로 해석할 수 있습니다. 따라서 `"EnableDnsHostnames": true` 단독으로는 DNS 해석 기능이 작동하지 않습니다. 반드시 `"EnableDnsSupport": true` 옵션이 함께 존재해야 합니다.

---

`"InstanceTenancy"`는 VPC 내부에 배치된 인스턴스가 작동하는 물리 서버의 tenancy를 설정합니다. `"default"`는 다른 사용자와 공유되는 물리 서버에 인스턴스를 배치하겠다는 의미입니다.

`"InstanceTenancy": "default"` 옵션은 VPC 레벨에서 적용됩니다. 따라서 VPC 내부의 개별 인스턴스를 공유 물리 서버가 아닌 전용 물리 서버(`"dedicated"`)로 바꿀 수 있습니다.  
만약 VPC 레벨에서 `"InstanceTenancy": "dedicated"` 옵션이 설정되어 있으면, VPC 내부 인스턴스는 공유 물리 서버로 바꿀 수 없습니다.

Tenancy에는 `"host"` 옵션도 존재합니다. `"dedicated"` 옵션을 통해 전용 물리 서버를 사용할 수 있지만, 특정 물리 서버를 선택할 수는 없습니다. 이 경우 AWS에서 배정해주는 전용 물리 서버를 사용할 수만 있습니다. 반면에 인스턴스에 `"host"` 옵션을 사용하면 물리 서버를 독점함과 동시에 특정 Dedicated Host를 선택할 수 있습니다. `"host"` 옵션은 VPC 레벨에서 `"InstanceTenancy": "default"`인 경우에도 사용할 수 있습니다.

---

`"Tags"`는 리소스에 달린 꼬리표 같은 기능을 하는 필드입니다. `"Tags"`의 `"Key"`와 `"Value"` 값을 바탕으로 특정 리소스들을 검색하거나, 같은 `"Key"` 또는 같은 `"Key"`-`"Value"` 조합을 가진 리소스들을 검색할 수 있습니다.

---

## 2. `"BasicRetailStoreVPCIGW448D34CF"`

```json
  "BasicRetailStoreVPCIGW448D34CF": {
   "Type": "AWS::EC2::InternetGateway",
   "Properties": {
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Name",
      "Value": "retail-store-vpc"
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

인터넷 게이트웨이(IGW)를 생성하는 리소스입니다.

---

## 3. `"BasicRetailStoreVPCVPCGW9E2A7641"`

```json
  "BasicRetailStoreVPCVPCGW9E2A7641": {
   "Type": "AWS::EC2::VPCGatewayAttachment",
   "Properties": {
    "InternetGatewayId": {
     "Ref": "BasicRetailStoreVPCIGW448D34CF"
    },
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  }
```

VPC Gateway Attachment 를 생성하는 리소스입니다. 

VPC Gateway Attachment는 IGW와 VPC를 연결하는 리소스입니다. 

---

`"InternetGatewayId"`와 `"VpcId"`는 `"Ref"`로 각 리소스의 Logical ID를 참조하여 연결합니다.

참고로 "Ref"는 AWS가 정해둔 리소스의 특정 값을 반환합니다. 여기서는 리소스의 Logical ID를 참조해 각각 실제 생성된 인터넷 게이트웨이 ID와 VPC ID를 반환합니다. 이 두 값은 AWS에서 실제 생성된 리소스가 가진 값으로, 이 템플릿 안에는 직접 존재하지 않는 값입니다.
