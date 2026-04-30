---
title: "Fundamentals Lab"
weight: 0
---
> **작성일:** 2026-04-29 | **수정일:** 2026-04-30

이번 랩에서는 ECS의 핵심 구성 요소인 ECS Cluster, Task Definition, ECS Service를 생성합니다. 목표는 Application Load Balancer(ALB) 뒤에서 동작하는 컨테이너를 배포하는 것입니다.

다음은 ECS의 핵심 구성 요소들에 대한 간단한 소개입니다. 
# Amazon ECS Core Components

![ECS Core Components](ecs-core-component.png)

**Cluster**: 서비스들과 태스크들을 모아놓은 논리적 집합.

**Service**: 동일한 태스크들의 집합. 

>여기서 동일한 태스크들을 여러 개 실행하는 이유는 대부분 Multi-AZ 구성을 통해 High Availability를 구현하거나, 트래픽을 분산하거나, 한 태스크에 장애가 발생했을 때 다른 태스크가 그 트래픽을 처리하도록 하기 위함입니다.

**Task**: 하나 이상의 컨테이너가 실행되는 단위.

>MSA 환경에서 하나의 태스크는 보통 하나의 애플리케이션 컨테이너를 실행합니다. 경우에 따라 로그 수집, 프록시, 모니터링 등을 담당하는 사이드카 컨테이너를 함께 실행하기도 합니다. 즉, 하나의 태스크 안에는 하나 이상의 컨테이너가 포함될 수 있습니다.

**Task Definition**: 컨테이너의 설계도.

> 컨테이너의 설계도인 Task Definition을 통해 컨테이너 이미지, CPU/메모리, 포트, 환경 변수, 로그 설정 등 컨테이너가 실행되는 방식을 정의합니다.

