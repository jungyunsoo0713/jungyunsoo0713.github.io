---
title: "01. 코어 네트워크 (VPC & 게이트웨이)"
weight: 1
---
> **작성일:** 2026-04-17 | **수정일:** 2026-04-20
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

---

`"BasicRetailStoreVPC0FA21CD5"`는 CloudFormation 템플릿의 Logical ID입니다. Logical ID는 클라우드 포메이션 템플릿 내부의 리소스들을 구별하는 식별자입니다. 즉, AWS 상에서 구분하기 위한 것이 아니라, 클라우드 포메이션 템플릿에서의 구별을 위한 식별자입니다.

이 Logical ID는 리소스 이름 뒤에 해시값인 `"0FA21CD5"`가 붙습니다. 이 해시값은 CDK가 construct path를 바탕으로, stack 이름을 제외하고 일부 경로 요소에 규칙을 적용한 뒤 계산한 값입니다.

예를 들면
```ts
new VPC(this, 'BasicRetailStoreVPC') // BasicRetailStoreVPC / Resource
```

이 타입스크립트 기반의 CDK 코드는 VPC를 생성하는 코드입니다. `Resource`는 CDK가 내부적으로 생성한 CloudFormation 리소스의 construct path에서 자주 보이는 이름입니다. 이 이름은 Logical ID의 사람이 읽는 앞부분에는 나타나지 않을 수 있지만, 해시 계산에는 반영될 수 있습니다. (`Resource`라는 키워드는 클라우드포메이션 템플릿의 Logical ID에 직접 나타나지는 않지만, 해시 계산에는 포함됩니다.)

`BasicRetailStoreVPC / Resource`와 같은 construct path를 바탕으로 해시값이 계산되며, 그 결과 Logical ID 뒤에 `"0FA21CD5"`와 같은 해시값이 붙습니다. 그렇다면 왜 굳이 부모 리소스의 이름까지 모두 더해서 해시값을 만들까요? 그 이유는 리소스의 Logical ID가 전체 스택 내에서 반드시 유일해야 하기 때문입니다.

```ts
new ServiceA(this, 'ServiceA')  // 내부에 new Database(this, 'Database')
new ServiceB(this, 'ServiceB')  // 내부에 new Database(this, 'Database')
```

예를 들어, `'ServiceA'`와 `'ServiceB'`가 각각 내부에 `'Database'`라는 이름을 가진 리소스를 생성한다고 가정해 봅시다. 만약 리소스 이름인 `'Database'`만으로 해시값을 생성하게 된다면, 두 리소스의 해시값이 동일해져서 Logical ID로서의 기능을 할 수 없습니다. 이 때문에 부모 리소스의 이름인 `'ServiceA'`와 `'ServiceB'`를 경로에 포함하여 각각 `ServiceA/Database`와 `ServiceB/Database`로 유일성을 만들어 주는 것입니다. 

---

CloudFormation 템플릿에서 리소스를 생성할 때 추가로 지정할 설정값이 있으면, 해당 리소스는 `"Properties"` 블록 안에 그 속성들을 정의합니다.

---

`"CidrBlock": "10.0.0.0/16"`은 이 VPC의 프라이빗 IP 주소 범위를 나타냅니다. `/16`은 앞의 16비트가 네트워크 부분이라는 뜻이므로, 이 VPC에서 사용할 수 있는 프라이빗 IP 주소 범위는 `10.0.0.0`부터 `10.0.255.255`까지입니다.

---

`"EnableDnsHostnames": true`는 public IPv4 주소가 있는 인스턴스에 퍼블릭 DNS hostname을 할당할 수 있게 하는 설정입니다. `"EnableDnsSupport"`가 `true`이면 VPC는 Amazon Route 53 Resolver를 통한 DNS 해석을 지원합니다. 또한 두 속성이 모두 `true`인 경우, public IPv4 주소가 있는 인스턴스는 퍼블릭 DNS hostname을 받을 수 있고, Amazon이 제공하는 private DNS hostname도 해석할 수 있습니다. 예를 들어, EC2 인스턴스는 `ec2-54-12-34-56.ap-northeast-2.compute.amazonaws.com`과 같은 퍼블릭 DNS hostname을 가질 수 있습니다.

>비유를 통해 말하자면, `"EnableDnsHostnames": true`는 public IPv4 주소가 있는 인스턴스에 퍼블릭 DNS hostname이라는 이름표를 붙일 수 있게 하는 역할을 하고, `"EnableDnsSupport": true`는 그 이름표를 실제 IP 주소로 찾아주는 전화번호부 역할을 합니다. 만약 `"EnableDnsSupport"`가 `false`라면, 인스턴스에 DNS 이름이 있더라도 그 이름을 IP 주소로 해석할 수 없으므로 도메인 네임을 통한 통신이 제대로 동작하지 않습니다. 쉽게 말해, 인스턴스가 자신의 IP에 대응되는 이름표를 달고 있더라도, 외부나 내부에서 그 이름표를 보고 실제 주소를 찾을 수 있는 전화번호부가 없으면 이름으로는 통신할 수 없는 것과 같습니다.

>인스턴스는 AWS 리소스의 한 종류로, 보통 하나의 독립된 서버처럼 동작하는 실행 단위를 가리킵니다. 예를 들어 EC2 인스턴스는 AWS의 가상 서버이고, RDS의 DB 인스턴스는 클라우드에서 실행되는 독립된 데이터베이스 환경입니다. 반면 ECS의 실행 단위는 보통 인스턴스가 아니라 태스크(task)라고 부릅니다. 또한 S3 버킷이나 IAM Role처럼 저장소나 권한 객체는 인스턴스라고 부르지 않습니다.

>IP 주소는 ENI에 할당됩니다. EC2 인스턴스는 ENI를 통해 네트워크에 연결되므로, 인스턴스의 IP 주소도 ENI에 할당됩니다.

---

`"InstanceTenancy": "default"`는 인스턴스를 생성할 때 하드웨어를 공유하는 방식(`default`)과 단독으로 사용하는 방식(`dedicated`) 중 무엇을 기본값으로 할지 결정합니다.

>VPC 레벨에서 `default`로 설정하면 내부 인스턴스들은 기본적으로 공유 하드웨어에서 실행되지만, 개별 인스턴스 설정에 따라 특정 서버만 `dedicated`로 지정할 수 있습니다. 반면, VPC 레벨에서 `"InstanceTenancy": "dedicated"`를 선택하면 내부의 모든 서버는 기본적으로 전용 하드웨어(`dedicated`)에서 실행됩니다. 개별 인스턴스를 공유 하드웨어(`default`)로 바꾸는 것은 불가능합니다. 다만 `host` tenancy는 별도로 지정할 수 있습니다. `host` tenancy는 특정 Dedicated Host(전용 물리 서버)를 지정해 그 위에 인스턴스를 실행하는 방식입니다. 즉, `dedicated`는 전용 하드웨어에서 실행되지만 어떤 물리 서버에 배치될지는 사용자가 직접 제어하지 않는 반면, `host`는 특정 물리 서버를 직접 지정할 수 있습니다.

---

`"Tags"`는 각 리소스의 꼬리표 같은 것으로, 리소스를 구분하거나 분류하고, 복잡한 리소스 이름으로 검색하는 대신 꼬리표의 값으로 검색 및 필터링 할 일이 있을 때 사용됩니다.

위 코드를 예로 들면:

```json
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
```

이 리소스에는 `CreationDate=2026-02-21`이라는 태그가 붙습니다. 생성 날짜가 `2026-02-21`인 리소스를 검색할 때, `Tags`의 `Key`와 `Value`를 필터링하여 해당 리소스를 찾을 수 있습니다.

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

---

인터넷 게이트웨이를 생성합니다. 인터넷 게이트웨이는 VPC와 인터넷 사이의 통신을 가능하게 하는 VPC 구성 요소입니다.

---

IGW의 Logical ID는 `"BasicRetailStoreVPCIGW448D34CF"`입니다.

이 코드를 보면
```ts
new VPC(this, 'BasicRetailStoreVPC')
```

`'BasicRetailStoreVPC'`는 VPC construct의 id이고, IGW는 CDK가 이 VPC construct 아래에서 내부적으로 생성한 관련 리소스입니다. 따라서 IGW의 construct path는 대략 `MyStack/BasicRetailStoreVPC/IGW/Resource` 형태가 됩니다. construct path에는 stack 이름이 포함되지만, Logical ID를 만들 때는 stack 이름이 제외됩니다.

CDK는 이 construct path를 바탕으로 Logical ID를 생성합니다. 이때 사람이 읽을 수 있는 앞부분은 `"BasicRetailStoreVPCIGW"`가 되며, 여기에 해시값이 붙어 `"BasicRetailStoreVPCIGW448D34CF"`와 같은 형태가 됩니다. 참고로 `Resource`는 사람이 읽을 수 있는 앞부분에서는 생략되지만, 해시 계산(`BasicRetailStoreVPC/IGW/Resource`)에는 포함됩니다.

같은 원리로 VPC 자체의 construct path는 대략 `MyStack/BasicRetailStoreVPC/Resource` 형태입니다. 다만 Logical ID를 만들 때는 stack 이름을 제외하므로, VPC의 경우에는 `BasicRetailStoreVPC/Resource`를 바탕으로 `"BasicRetailStoreVPC"` 뒤에 해시값이 붙는 형태가 됩니다.

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

---

VPC Gateway Attachment 리소스를 생성합니다. AWS 특정 리소스는 생성과 연결을 따로 해주어야 하며, 이 연결 자체가 하나의 리소스로 취급됩니다. VPC Gateway Attachment는 생성된 Internet Gateway를 VPC에 연결해 줍니다. 따라서 `"Properties"`에서 `"InternetGatewayId"`와 `"VpcId"`를 각각 참조(`"Ref"`)합니다.

`"Ref"`는 리소스마다 AWS가 정해 둔 기본 반환값을 반환합니다. 따라서 어떤 리소스에서는 퍼블릭 IP를 반환할 수 있지만, 다른 리소스에서는 VPC ID, Subnet ID, NAT Gateway ID 같은 식별자를 반환할 수 있습니다. 여기서는 `"BasicRetailStoreVPCIGW448D34CF"`의 `"Ref"`가 Internet Gateway ID를, `"BasicRetailStoreVPC0FA21CD5"`의 `"Ref"`가 VPC ID를 반환하므로, 그 값을 이용해 Internet Gateway를 VPC에 연결합니다.
