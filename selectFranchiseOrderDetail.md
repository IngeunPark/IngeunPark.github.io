---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 발주 내역 상세 조회

<p style="font-weight:bold">가맹점이 신청한 <font style="color: red;">발주 내역의 상세 내용</font>을 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_1.png"> <br>

! 제가 클라이언트라면 [가맹점 발주 내역 상세 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사와 가맹점이 가맹점 발주 내역 상세 조회를 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_3.png"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였습니다.
! 쿼리 메소드 findById를 사용하여 가맹점 발주 내역 Entity를 조회한 후 컨트롤러에 DTO로 변환하여 반환하도록 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_4.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_5.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_6.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_7.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다. 비동기 기능이기 때문에 contentType에 APPLICATION_JSON을 사용하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 가맹점 발주 상세 조회를 위해 given에서 가맹점 발주 내역 번호를 제공합니다.
 2. when에서 가맹점 발주 내역 번호를 통해 쿼리 메소드 findById를 사용하여 가맹점 발주 내역 Entity인 FranchiseOrder 자료형 변수에 저장합니다. 
 3. 그 후 then에서 assertNotNull을 통해 가맹점 발주 내역 정보와 가맹점 발주 내역 신청 물품 정보가 Null이 아닌지 확인하며 테스트를 마무리하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 발주 내역 상세 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 발주 내역 상세 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_8.png"> <br>
-> 조회할 가맹점 발주 내역을 클릭합니다. 클릭 시 상세 조회 모달창을 조회할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_9.png"> <br>
-> 가맹점 발주 내역 상세 조회 모달창입니다. 발주 신청 물품 이름, 개당 금액, 수량, 물품 당 총 금액을 조회할 수 있습니다.

#### 코드

##### [Html]
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_10.png"> <br>
-> 상세 조회 모달창 코드입니다. 미리 틀을 만들어 둔 후에 script를 통해 tbody에 내용을 추가하도록 구현하였습니다. <br>

##### [JavaScript]
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_11.png"> <br>
-> 상세 조회하기 위한 비동기 fetch 코드입니다. 비동기 작업 수행 성공 시 변수에 Controller에서 받은 정보를 태그로 감싸 담고 JQuery를 사용하여 tbody 태그의 자식으로 생성하도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_12.png"> <br>
-> view에서 전달한 발주 내역 번호를 WebRequest를 사용하여 변수에 담고 service 메소드에 전달하였습니다. 반환 받은 발주 내역 물품 목록 정보를 view에 gson을 사용하여 반환하도록 구현하였다.

##### [Service]
<img src="../../../../img/jaegojaego/franchiseOrderDetail/franchise-order-detail_13.png"> <br>
-> Contoller에서 제공 받은 가맹점 발주 내역 번호로 쿼리 메소드 findById 를 사용하여 가맹점 발주 내역 상세 정보를 조회하였습니다. 그 후 필요한 정보를 담기 위해 FranchiseOrderDetailDTO 자료형 List
에 반복문을 통하여 정보를 저장 후 Controller로 반환하도록 구현하였습니다.


## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



