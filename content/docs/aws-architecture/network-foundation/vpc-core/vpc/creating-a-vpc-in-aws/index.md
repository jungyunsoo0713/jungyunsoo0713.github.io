---
title: "Creating a VPC in AWS"
date: 2026-04-16
lastmod: 2026-04-16
weight: 2
---
> **작성일:** 2026-04-16 | **수정일:** 2026-04-16
# AWS에서 VPC 생성하기

이번 포스트에서는 AWS 콘솔에서 실제로 VPC를 생성해보겠습니다.

## 1. Amazon VPC 콘솔로 이동하기

AWS에 로그인한 뒤, 상단 검색창에서 `VPC`를 검색해 Amazon VPC 콘솔로 이동합니다.

![[vpc-01.png]]

## 2. `Create VPC` 버튼 클릭하기

`Create VPC` 버튼을 눌러 VPC 생성 화면으로 이동합니다.

![[vpc-02.png]]

## 3. `VPC only` 선택하기

`Resources to create` 항목에서 `VPC only`를 선택합니다. 이번 실습에서는 VPC만 생성할 것이므로 이 옵션을 선택합니다.

## 4. VPC 이름과 IPv4 CIDR 블록 입력하기  
  
`Name tag`에 VPC 이름을 입력합니다. 그리고 `IPv4 CIDR block`에는 사용할 사설 IP 주소 범위를 입력합니다. 이번 예시에서는 `10.0.0.0/16`을 사용하겠습니다. 나머지 설정은 변경하지 않습니다.
  
![[vpc-03.png]]

## 5. `Create VPC` 버튼 클릭하기

모든 설정을 입력했다면 화면 하단의 `Create VPC` 버튼을 클릭합니다.

![[vpc-04.png]]

## 6. 생성된 VPC 확인하기

VPC 생성이 완료되면 결과 화면이 나타납니다. 이후 VPC 목록에서 방금 생성한 VPC가 보이는지 확인합니다. 이때 `Name`과 `IPv4 CIDR` 값이 입력한 내용과 일치하는지도 함께 확인합니다.

![[vpc-05.png]]

## 마무리

지금까지 AWS 콘솔에서 VPC를 생성해보았습니다. 이번 실습을 통해 VPC 생성 과정과 기본 설정 항목을 확인할 수 있었습니다.
