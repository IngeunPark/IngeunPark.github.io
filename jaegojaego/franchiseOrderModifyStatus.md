---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 발주 내역 처리 상태 변경

<p style="font-weight:bold">가맹점이 발주 내역의 <font style="color: red;">처리 상태를 변경</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_1.png"> <br>

! 제가 클라이언트라면 [가맹점 발주 내역 처리 상태 변경] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 가맹점 발주 내역의 처리 상태를 변경할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_3.png"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였습니다. <br>
! 쿼리 메소드 findById를 사용하여 처리 상태를 변경할 발주 내역을 조회한 후 setter를 사용하여 처리 상태를 변경하도록 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_4.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_5.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_6.png"> <br>

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 가맹점 발주 내역 처리 상태를 위해 given에서 변경할 발주 내역 번호와 변경할 처리 상태를 제공합니다. 또한 승인거부의 경우 발주 거부 사유서 작성 내용을 제공합니다.
 2. when에서 가맹점 발주 내역 번호를 통해 쿼리 메소드 findById를 사용하여 변경할 발주 내역을 조회합니다.
 3. 변경할 처리 상태, 처리 일자, 처리자를 setter로 저장합니다. 
 4. then에선 처리 상태가 변경되었는지 assertTrue로 확인하고, 발주 내역이 잘 조회되는지 assertNotNull로 확인하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 발주 내역 처리 상태 변경에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 발주 내역 처리 상태 변경 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_7.png"> <br>
-> 변경할 가맹점 발주 내역의 처리 상태를 클릭하여 변경할 발주 내역 버튼을 조회할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_8.png"> <br>
-> 버튼 클릭 시 처리 상태 변경 확인 모달창이 조회됩니다.

<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_9.png"> <br>
-> 처리 상태 변경 확인 모달창입니다. Ok 버튼 클릭 시 처리 상태 변경이 완료되며 승인거부의 경우 발주 거부 사유서를 작성할 수 있습니다.

<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_10.png"> <br>
-> 발주 거부 사유서 모달창입니다. 내용을 작성할 수 있습니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_11.png">
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_12.png">
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_13.png"><br>
-> 버튼 클릭 시 처리 상태를 변경하는 코드입니다. fetch 비동기 방식을 사용하여 Controller에 발주 내역 번호와 처리 상태를 전달하고 성공 시 flag변수를 사용하여 처리 상태 변경을 알리도록 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_14.png"> <br>
-> view에서 전달한 발주 내역 번호, 처리 상태, 거절 메세지를 WebRequest를 사용하여 변수에 담고 service 메소드에 전달하였습니다. 상태 변경 성공 시 view에 성공 메세지를 반환하도록 구현하였습니다.

##### [Service]
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_15.png">
<img src="../../../../img/jaegojaego/franchiseOrderModifyStatus/franchise-order-modify-status_16.png"><br>
-> Contoller에서 제공 받은 가맹점 발주 내역 번호로 쿼리 메소드 findById 를 사용하여 가맹점 발주 내역을 조회하여 Entity 변수에 저장합니다. 
그 후 setter를 사용하여 처리 상태, 처리자, 처리 일자, 거절 메시지를 변경하여 저장하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



