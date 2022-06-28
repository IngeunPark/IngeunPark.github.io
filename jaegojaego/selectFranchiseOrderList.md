---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 발주 내역 목록 조회

<p style="font-weight:bold">가맹점이 신청한 <font style="color: red;">발주 내역의 목록</font>을 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_1.png"> <br>

! 제가 클라이언트라면 [가맹점 발주 내역 목록] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사와 가맹점이 가맹점 발주 내역 목록을 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

## [자세한 내용](https://www.notion.so/912b85f8f7f645b6859401cccae0124b)

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_3.png"> <br>

? 기본적으로 가맹점 발주는 본사가 가공한 자재에 대해 발주를 신청할 수 있는 기능입니다. <br>
? 하지만 초기 논리 모델링에서는 자재 가공에 대해 생각하지 못하고 본사와 가맹점이 같은 자재에 대해 다루는 문제가 존재하였습니다. 또한 발주와 수주가 같은 기능을 하는 테이블로
컬럼이 같지만 테이블을 분리하여 문제가 발생하였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_4.png"> <br>

! 최종적으로는 본사가 가공한 자재를 선택할 수 있도록 가맹점 주문 가능 물품이란 테이블을 만들어 사용하였고 변경 이력 테이블 또한 만들어 처리 상태 변경을 관리하였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_5.png">
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_6.png">
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_7.png"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였습니다. <br>
! 가맹점 발주 내역 목록 조회에는 3가지 조건이 존재하며 각 조건에 맞게 시퀀스 다이어그램을 작성하였습니다.
 1. 본사 직원은 본인이 담당 중인 가맹점 계정이 신청한 발주 내역 목록을 조회할 수 있습니다.
 2. 가맹점 대표는 본인이 신청한 발주 내역과 같은 가맹점의 매니저가 신청한 발주 내역의 목록을 조회할 수 있습니다.
 3. 가맹점 매니저는 본인이 신청한 발주 내역과 같은 가맹점의 대표가 신청한 발주 내역의 목록을 조회할 수 있습니다. <br><br>
! 조건에 맞게 가맹점 대표와 가맹점 매니저의 정보를 조회한 후 신청한 발주 내역의 목록을 조회할 수 있도록 시퀀스 다이어그램을 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_8.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_9.png"> <br>

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 로그인한 사용자가 본사라는 조건으로 테스트를 진행하여 본사 계정 번호, 계정 구분에 대한 정보를 제공하고 조건에 따라 가맹점 정보를 담을 List 변수들을 제공하였습니다.
 2. when에서 본사 직원이 담당 중인 가맹점 대표의 정보를 쿼리 메소드를 사용하여 조회한 후 가맹점 대표 목록 정보를 토대로 가맹점 매니저 목록 정보를 조회합니다.
 3. 대표 목록 정보와 매니저 목록 정보를 사용하여 신청한 발주 내역 목록을 각 List에 담고 하나의 List에 저장하도록 구현하였습니다. 
 4. 그 후 then에서 assertNotNull을 통해 가맹점 발주 내역 목록이 저장되었는지 확인하며 마무리하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 발주 내역 목록 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 발주 내역 목록 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_10.png"> <br>
-> 본사 계정으로 로그인 후 좌측 가맹점 발주 관리 메뉴의 하위 메뉴인 발주 내역 메뉴를 클릭 시 가맹점 발주 내역 목록 조회 페이지로 이동합니다. 조회되는 내용을 토대로 검색이 
가능하며 한 페이지 당 최대 8까지 조회되도록 하였습니다. 본사 계정은 담당 중인 가맹점이 신청한 발주 내역의 목록을 조회할 수 있도록 구현하였습니다. 신청 가맹점 이름, 주문번호
, 신청 일자, 신청자, 처리 일자, 처리자가 조회되도록 구현하였습니다. <br>

#### 코드

##### [Html]
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_11.png"> <br>
-> thymeleaf를 사용하여 Controller에서 전달 받은 가맹점 발주 내역 정보를 화면에 출력되도록 구현하였습니다. 
th:if 태그를 사용하여 대표가 신청한 발주인지 매니저가 신청한 발주인지 조건을 따지도록 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_12.png"> <br>
-> 로그인 한 사용자가 본사 계정인지 가맹점인지, 가맹점이라면 대표인지 매니저인지 정보가 필요하기 때문에 로그인 한 사용자의 정보를 얻기 위해 Authentication 을 사용하여 변수에 저장하였습니다.
service 메소드에 로그인한 사용자의 정보를 전달하고 가맹점 발주 내역 목록을 반환 받고 sorted를 사용하여 최신순으로 정렬 후 view에 전달하도록 구현하였습니다.

##### [Service]
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_13.png"> <br>
-> 본사 직원의 경우입니다. Controller에서 전달 받은 정보를 토대로 본사 직원 계정인지 if 문으로 조건을 따진 후 담당 중인 가맹점 대표 목록과 매니저 목록을 변수에 저장합니다. 
setFranchiseOrderList 메소드를 사용하여 각 가맹점이 신청한 발주 내역을 정리 후 Controller로 반환하도록 구현하였습니다. 

<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_14.png">
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_15.png"> <br>
-> setFranchiseOrderList 메소드입니다. 제공 받은 정보를 토대로 발주 내역의 신청자 번호와 대표자 번호 또는 매니저 번호와 같을 경우 List에 발주 내역을 저장하도록 구현하였습니다.
또한 처리일자가 존재할 경우 처리 정보 또한 담아 저장하도록 구현하였습니다.

<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_16.png">
<img src="../../../../img/jaegojaego/franchiseOrderList/franchise-order-list_17.png"> <br>
-> 가맹점 계정의 경우입니다. 가맹점 계정의 경우 전달 받은 정보를 토대로 대표인지 매니저인지 조건을 따지도록 하였습니다. 대표의 경우 대표의 정보를 토대로 매니저 정보를 조회하고 
매니저의 경우 대표의 정보를 조회 후 그 정보를 이용하여 다른 매니저 목록을 불러오도록 하였습니다. 최종적으로 가맹점 계정들의 정보를 setFranchiseOrderListByFranchise 메소드에 전달하여
가맹점 발주의 정보를 정리, 조회 후 반환 받은 값을 Controller에 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



