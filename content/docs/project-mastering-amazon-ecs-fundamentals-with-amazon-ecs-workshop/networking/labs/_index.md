---
title: "Networking - Labs"
weight: 2
---
> **작성일:** 2026-05-10 | **수정일:** 2026-05-10

다음은 Lab 실습에 사용하는 Retail Store Sample App 네트워크 구조의 일부입니다.
![Microservices Architecture](microservices-architecture.svg)

`UI -> Orders` 구간에서 `http://orders`는 UI 서비스가 Orders 서비스의 API를 호출하는 것을 의미합니다. 주문 목록 조회, 주문 상세 조회 등의 API를 호출합니다.

`UI -> Checkout` 구간에서 `http://checkout`은 UI 서비스가 Checkout 서비스의 API를 호출하는 것을 의미합니다. 체크아웃 과정에서의 고객 데이터(주문자 정보, 배송지, 배송 옵션 등) 저장 등의 API를 호출합니다.

`Checkout -> Orders` 구간에서 `http://orders`는 Checkout 서비스가 Orders 서비스의 API를 호출하는 것을 의미합니다. 주문 생성 관련 API를 호출합니다.

`UI -> Catalog` 구간에서 `http://catalog`는 UI 서비스가 Catalog 서비스의 API를 호출하는 것을 의미합니다. 상품 목록 조회, 상품 상세 조회 등의 API를 호출합니다.

UI 서비스를 제외한 나머지 Orders, Checkout, Catalog 서비스는 각각 자신만의 저장소를 가집니다. 다른 서비스로부터 요청이 올 때 필요한 경우 이 저장소에 접근하여 필요한 데이터를 가져올 수 있습니다.

---

다음 세 개의 랩에서는 Fargate와 관련된 Amazon ECS 네트워킹 개념을 살펴봅니다.
