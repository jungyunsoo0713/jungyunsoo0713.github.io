---
title: "ECS Service Connect"
weight: 0
---

ECS Service Connect는 ECS 서비스끼리 통신하게 해주는 설정입니다. Service Connect를 사용하면 각 서비스가 서로의 Task IP를 직접 알 필요 없이, `orders`, `catalog`, `checkout` 같은 서비스 이름으로 다른 서비스를 호출할 수 있습니다.

예를 들어 UI 서비스는 Orders 서비스의 실제 Task IP를 몰라도 `http://orders`로 요청을 보낼 수 있습니다. 이 이름은 Service Connect가 관리하는 내부 이름이며, 요청은 Service Connect 프록시를 거쳐 실제 Orders 서비스의 Task로 전달됩니다.

즉, Service Connect는 서비스 간 통신에 사용할 내부 이름을 만들고, 그 이름으로 들어온 요청을 실제 ECS Task로 연결해주는 역할을 합니다.
