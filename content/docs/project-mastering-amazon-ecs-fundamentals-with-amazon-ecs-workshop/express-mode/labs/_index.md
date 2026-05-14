---
title: "Express Mode"
weight: 1
---
Amazon ECS Express Mode는 AWS가 복잡한 인프라를 대신 설정하여 컨테이너 서비스를 배포해주는 ECS의 기능입니다. 

ECS Express Mode 서비스는 메인 컨테이너(Primary Container) 이미지와 Task Execution Role, Infrastructure Role을 지정하면, 나머지 인프라는 AWS가 이에 맞추어 자동으로 인프라를 설정하고 배포해줍니다.

이 모드에서 MSA 아키텍처를 구현하고 싶다면 여러 개의 ECS Express Mode 서비스를 생성해야 합니다. 네트워크 설정을 통해 여러 Express Mode 서비스가 같은 VPC, 서브넷, 보안 그룹 등의 리소스를 공유할 수 있기 때문에 이것이 가능합니다. 보조 컨테이너를 추가하고 싶다면, Task Definition을 직접 수정해서 사이드카 컨테이너를 추가할 수 있습니다.

AWS 콘솔이나 CLI로 Express Mode 서비스를 생성한 경우, 이 서비스는 CloudFormation 스택으로 관리되지 않습니다. 다만 IaC(CloudFormation, Terraform 등)를 사용해 Express Mode 서비스를 관리할 수 있습니다. 하지만 각 리소스를 세밀하게 관리하는 방식이 아니라, Express Mode 서비스의 설정값 등을 관리하는 수준입니다.
