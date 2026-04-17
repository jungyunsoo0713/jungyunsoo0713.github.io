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
# 1. `"BasicRetailStoreVPC0FA21CD5"`

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

이 Logical ID는 리소스 이름 뒤에 해시값인 `"0FA21CD5"`가 붙습니다. 이 해시값은 CDK 코드에서 리소스와 그 부모들의 이름을 붙인 값을 해시 함수에 넣어서 만들어집니다.

예를 들면
```ts
new VPC(this, 'BasicRetailStoreVPC') // MyStack / BasicRetailStoreVPC / Resource
```

이 타입스크립트 기반의 CDK 코드는 VPC를 생성하는 코드입니다. 여기서 `this`는 `'BasicRetailStoreVPC'`의 부모 리소스인 스택(`MyStack`)을 의미합니다. `Resource`는 CDK가 클라우드포메이션의 각 리소스(VPC, IGW, NAT GW 등)에 붙이는 이름입니다. (`Resource`라는 키워드는 클라우드포메이션 템플릿의 Logical ID에 직접 나타나지는 않지만, 해시 계산에는 포함됩니다.)

`MyStack / BasicRetailStoreVPC / Resource`라는 전체 경로 문자열을 해시 함수에 통과시켜서 얻은 값이 바로 `"0FA21CD5"`입니다. 그렇다면 왜 굳이 부모 리소스의 이름까지 모두 더해서 해시값을 만들까요? 그 이유는 리소스의 Logical ID가 전체 스택 내에서 반드시 유일해야 하기 때문입니다.

```ts
new ServiceA(this, 'ServiceA')  // 내부에 new Database(this, 'Database')
new ServiceB(this, 'ServiceB')  // 내부에 new Database(this, 'Database')
```

예를 들어, `'ServiceA'`와 `'ServiceB'`가 각각 내부에 `'Database'`라는 이름을 가진 리소스를 생성한다고 가정해 봅시다. 만약 리소스 이름인 `'Database'`만으로 해시값을 생성하게 된다면, 두 리소스의 해시값이 동일해져서 Logical ID로서의 기능을 할 수 없습니다. 이 때문에 부모 리소스의 이름인 `'ServiceA'`와 `'ServiceB'`를 경로에 포함하여 각각 `ServiceA/Database`와 `ServiceB/Database`로 유일성을 만들어 주는 것입니다.

---

CloudFormation 템플릿의 리소스 중 설정할 속성이 있는 리소스들은 `"Properties"`를 가집니다.

---

`"CidrBlock": "10.0.0.0/16"`은 이 VPC의 프라이빗 IP 주소 범위를 나타냅니다. 앞의 16비트가 네트워크 주소를 식별하는 데 쓰이기 때문에, 이 프라이빗 IP 주소의 범위는 `10.0.0.0`부터 `10.0.255.255`까지입니다.

---

`"EnableDnsHostnames": true`는 이 VPC 안에 IP 주소를 가진 리소스들이 AWS가 제공하는 기본 도메인 네임을 가질 수 있게 해줍니다. 예를 들어, EC2 인스턴스의 경우 `ec2-54-12-34-56.ap-northeast-2.compute.amazonaws.com`과 같은 퍼블릭 도메인 네임을 할당받습니다. 또한 VPC 내부 통신용인 프라이빗 DNS도 자동으로 발급해 줍니다.

---

`"EnableDnsSupport": true`는 이 VPC 내부에 DNS 서버를 설치하는 것입니다. 이는 `"EnableDnsHostnames": true`가 정상적으로 작동하기 위해 반드시 필요합니다.

즉, `"EnableDnsHostnames": true`는 리소스의 IP 주소에 이름표를 발급하는 역할을 하고, `"EnableDnsSupport": true`는 그 이름표를 찾아주는 전화번호부 역할을 합니다. 만약 `"EnableDnsSupport"`가 `false`라면, IP 주소에 매칭되는 이름표를 찾을 수 있는 서비스가 없으므로 도메인 네임을 사용할 수 없게 됩니다.

쉽게 설명하자면, 리소스가 자신의 IP와 이름표를 달고 있다고 해도, 외부나 내부에서 해당 이름표로 통신하려 할 때 전화번호부가 없으면 실제 IP와 매칭할 수 없는 것과 같습니다.

---

`"InstanceTenancy": "default"`는 인스턴스를 생성할 때 하드웨어를 공유하는 방식(`default`)과 단독으로 사용하는 방식(`dedicated`) 중 무엇을 기본값으로 할지 결정합니다.

VPC 레벨에서 `default`로 설정하면 내부 인스턴스들은 기본적으로 공유 하드웨어에서 실행되지만, 개별 인스턴스 설정에 따라 특정 서버만 `dedicated`로 지정할 수 있습니다. 반면, VPC 레벨에서 `"InstanceTenancy": "dedicated"`를 선택하면 내부의 모든 서버는 개별 설정과 관계없이 강제로 전용 하드웨어(`dedicated`)에 할당됩니다. 개별 인스턴스를 공유 하드웨어로 바꾸는 것은 불가능합니다.

---

`"Tags"`는 각 리소스의 꼬리표 같은 것으로, 복잡한 리소스 이름으로 검색하는 대신 꼬리표의 값으로 검색할 일이 있을 때 사용됩니다.

위 코드를 예로 들면:

```json
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
```

생성 날짜가 `2026-02-21`인 리소스를 검색할 때, `Tags`의 `Key`와 `Value`를 필터링하여 해당 리소스를 찾을 수 있습니다.

---
# 2. `"BasicRetailStoreVPCIGW448D34CF"`

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

Internet Gateway를 생성하는 코드입니다.

---

IGW의 Logical ID는 `"BasicRetailStoreVPCIGW448D34CF"`입니다.

이 코드를 보면
```ts
new VPC(this, 'BasicRetailStoreVPC')
```

`'BasicRetailStoreVPC'`는 VPC construct의 id이고, IGW는 CDK가 이 VPC construct 아래에서 내부적으로 생성한 관련 리소스입니다. 따라서 IGW의 construct path는 대략 `MyStack/BasicRetailStoreVPC/IGW/Resource` 형태가 됩니다.

CDK는 이 construct path를 바탕으로 Logical ID를 생성합니다. 이때 사람이 읽을 수 있는 앞부분은 `"BasicRetailStoreVPCIGW"`가 되며, 여기에 해시값이 붙어 `"BasicRetailStoreVPCIGW448D34CF"`와 같은 형태가 됩니다. 참고로 `Resource`는 사람이 읽을 수 있는 앞부분에서는 생략되지만, 해시 계산(`MyStack/BasicRetailStoreVPC/IGW/Resource`)에는 포함됩니다.

같은 원리로 VPC 자체의 construct path는 대략 `MyStack/BasicRetailStoreVPC/Resource` 형태이며, VPC의 Logical ID 역시 `"BasicRetailStoreVPC"`에 해시값이 붙은 형태로 만들어집니다.

---
# 3. `"BasicRetailStoreVPCVPCGW9E2A7641"`

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

VPC Gateway Attachment 리소스를 생성합니다. AWS 리소스는 생성과 연결을 따로 해주어야 하며, 이 연결 자체가 하나의 리소스로 취급됩니다. VPC Gateway Attachment는 생성된 Internet Gateway를 VPC에 연결해 줍니다. 따라서 `"Properties"`에서 `"InternetGatewayId"`와 `"VpcId"`를 각각 참조(`"Ref"`)합니다.

`"Ref"`는 리소스마다 AWS가 정해 둔 기본 반환값을 참조합니다. 따라서 어떤 리소스에서는 퍼블릭 IP를 반환할 수 있지만, 다른 리소스에서는 VPC ID, Subnet ID, NAT Gateway ID 같은 식별자를 반환할 수 있습니다.
