---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 본사 발주 내역 목록 조회

<p style="font-weight:bold">본사가 신청한 <font style="color: red;">발주 내역</font>의 목록을 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_1.png"> <br>

! 제가 클라이언트라면 [본사 발주 내역 목록] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 발주 내역 목록을 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_3.png"> <br>

? 기본적으로 본사 발주는 각 거래처 별 계약 물품에 대해 발주를 신청할 수 있는 기능입니다. <br>
? 하지만 초기 논리 모델링에서는 거래처 별 물품을 고려하지 않고 같은 물품으로 취급하고 설계를 하여 문제가 존재하였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_4.png"> <br>

! 최종적으로는 거래처 별 계약 물품으로 연관되도록 설계하였고 거래처 별 신청서 테이블 또한 설계하여 완성도를 높였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_5.png"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 쿼리 메소드를 사용하여 본사 발주 내역 전체를 조회하도록 설계하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_6.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_7.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_8.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_9.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 본사 발주 내역 번호의 Sequence를 통해 정렬할 수 있도록 given에서 제공합니다.
 2. when에서 쿼리 메소드를 통해 불러온 정렬된 본사 발주 내역 번호를 변수로 저장합니다.
 3. 그 후 then에서 assertNotNull을 통해 본사 발주 내역이 제대로 불러왔는지 확인하여 테스트를 마무리하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 본사 발주 내역 목록 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 본사 발주 내역 목록 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_10.png"> <br>
-> 본사 계정으로 로그인 후 좌측 본사 발주 관리 메뉴의 하위 메뉴인 발주 내역 메뉴를 클릭 시 본사 발주 내역 목록 조회 페이지로 이동합니다. 조회되는 내용을 토대로 검색이 가능하며 한 페이지 당 최대 8까지 조회되도록 하였습니다. 신청 일자, 신청자, 신청 물품, 총 금액, 처리 일자, 처리자, 처리 상태를 조회할 수 있습니다. <br>

#### 코드

##### [Html]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_11.png"> <br>
-> thymeleaf를 사용하여 Controller에서 전달 받은 본사 발주 내역 정보를 화면에 출력되도록 구현하였습니다. 물품의 경우 2개 이상일 때 [물품] 외 [개수] 로 표현되도록 구현하였으며 처리자가 존재 할 경우에 화면에 표시되도록 if문을 사용하여 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_12.png"> <br>
-> service 메소드를 통해 전체 발주 내역 목록을 불러와 List에 담고 발주 신청 물품의 개수에 따라 화면에 다르게 표시해 주어야 하기 때문에 조건을 따져 List에 저장되도록 구현하였습니다. <br>

<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_13.png"> <br>
-> 본사가 발주 신청할 때 신청 물품에 대한 거래처 별로 물품 가격이 다르기 때문에 발주 물품 Entity의 발주 내역 번호와 비교하여 가격을 계산하여 List 변수에 저장하도록 구현하였습니다. <br>

##### [Service]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_14.png"> <br>
-> 쿼리 메소드인 findAll을 통해 본사 발주 내역 목록 전체를 불러 온 후 List에 담고 modelMapper를 통해 Entity를 DTO 자료형으로 변환하여 반환하도록 구현하였습니다. <br>

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#first)



