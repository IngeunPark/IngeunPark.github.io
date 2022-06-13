---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 발주 거부 사유서 조회

<p style="font-weight:bold">가맹점 발주 내역 중 <font style="color: red;">승인 거절된 발주 내역의 발주 거부 사유서</font>를 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_1.png"> <br>

! 제가 클라이언트라면 [가맹점 발주 거부 사유서 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사와 가맹점이 가맹점 발주 내역 거부 사유서를 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_3.png"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였습니다.
! 쿼리 메소드 findById를 사용하여 가맹점 발주 내역 Entity를 조회한 후 발주 거부 사유서를 반환하도록 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_4.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_5.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_6.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_7.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다. 비동기 기능이기 때문에 contentType에 APPLICATION_JSON을 사용하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 가맹점 발주 상세 조회를 위해 given에서 가맹점 발주 내역 번호를 제공합니다.
 2. when에서 가맹점 발주 내역 번호를 통해 쿼리 메소드 findById를 사용하여 가맹점 발주 내역 Entity인 FranchiseOrder 자료형 변수에 저장합니다. 
 3. 그 후 then에서 assertNotNull을 통해 가맹점 발주 내역 거부 사유서가 Null이 아닌지 확인하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 발주 거부 사유서 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 발주 거부 사유서 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로 [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_8.png"> <br>
-> 조회할 가맹점 발주 내역 중 처리 상태가 '승인거절'인 발주 내역의 처리 상태를 클릭하여 발주 거부 사유서 모달창을 조회할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_9.png"> <br>
-> 가맹점 발주 거부 사유서 모달창입니다. 본사 직원이 작성한 발주 거부 사유 내용을 조회할 수 있습니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_10.png"> <br>
-> 가맹점 발주 거부 사유서를 조회하기 위해 fetch 비동기 방식을 사용하였습니다. 성공했을 경우 태그의 InnerText를 사용하여 값이 출력되도록 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_11.png"> <br>
-> view에서 전달한 발주 내역 번호를 WebRequest를 사용하여 변수에 담고 service 메소드에 전달하였습니다. 반환받은 발주 거부 사유서를 view에 전달하도록 구현하였습니다.

##### [Service]
<img src="../../../../img/jaegojaego/franchiseOrderRejectMessage/franchise-order-reject-message_12.png"> <br>
-> Contoller에서 제공 받은 가맹점 발주 내역 번호로 쿼리 메소드 findById 를 사용하여 가맹점 발주 내역 정보를 조회하였습니다. 그 후 발주 거부 사유서를 문자열 변수에 담고 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



