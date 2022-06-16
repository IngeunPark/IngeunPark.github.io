---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 발주 신청

<p style="font-weight:bold">가맹점이 물품을 선택해 <font style="color: red;">발주를 신청</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_1.png"> <br>

! 제가 클라이언트라면 [가맹점 발주 신청] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사와 가맹점이 가맹점 발주를 신청 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_3.png"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였습니다.
! 가맹점 발주 신청 시 가맹점 발주와 가맹점 발주 신청 물품 데이터를 저장해야 하기 때문에 발주 신청 데이터 저장 후 번호를 조회하여 그 번호를 사용하여 발주 신청 물품 데이터, 처리 상태 변경 이력
데이터를 저장하도록 시퀀스 다이어그램을 작성하였습니다.
! 신청하는 발주 내역의 주문 번호가 이전 발주 내역과 주문 번호가 겹치지 않도록 하기 위해 발주 내역을 전체 조회하도록 하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_4.png"> <br>
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_5.png"> 
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_6.png"> 
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_7.png"> <br>

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 저장될 주문 번호, 신청한 물품 번호, 수량을 given에서 제공하였습니다. 또한 발주 신청 시 기본적으로 자동으로 들어갈 신청 일자, 신청자, 처리 상태가 set 된 가맹점 발주 내역 Entity를 제공하였습니다.
 2. when에서 가맹점 발주 데이터를 저장한 후 저장된 발주 내역 번호를 조회하여 발주 신청 물품 데이터와 발주 처리 상태 변경 이력 데이터를 저장하도록 구현하였습니다.
 3. 발주 신청 물품 테이블은 복합 식별자를 사용하기 때문에 각 Entity를 선언 후 setter를 사용하여 값을 저장하고 최종적으로 물품 Entity에 setter를 사용하도록 구현하였습니다.
 4. 그 후 then에서 assertNotNull를 사용하여 각 발주 내역, 발주 신청 물품, 발주 처리 상태 변경 이력 테이블에 데이터가 저장되었는지 확인하였습니다.
  
#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 발주 신청에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 발주 신청 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_8.png"> <br>
-> 가맹점 계정은 가맹점 발주 내역 목록 페이지의 우측 상단에 발주 신청 버튼을 조회할 수 있습니다. 버튼을 클릭 시 가맹점 발주 신청 페이지로 이동할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_9.png"> <br>
-> 가맹점 발주 신청 페이지입니다. 가맹점이 발주 신청 가능한 물품을 조회할 수 있으며 물품 사진, 이름, 설명, 개당 금액을 조회할 수 있습니다. 또한 이름, 내역 금액 등으로 물품을
검색할 수 있습니다. 수량을 입력 후 '발주 물품에 추가' 버튼을 클릭 시 우측 발주 물품 목록에 추가됩니다. '메뉴 보기' 버튼 클릭 시 메뉴 페이지가 새 창에 열립니다. '발주' 버튼을 
클릭 하여 발주를 신청할 수 있습니다.

#### 코드

##### [Html]
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_10.png">
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_11.png"><br>
-> thymeleaf를 사용하여 Controller에서 전달 받은 가맹점 발주 신청 가능 물품의 정보를 화면에 표시되도록 구현하였습니다. 
발주 물품 추가 시 장바구니 처럼 사용하기 위해 div 태그를 사용하여 구현하였습니다. Script에서 물품 추가 시 tbody에 추가되도록 구현하였습니다. <br>

##### [JavaScript]
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_12.png"><br>
-> '발주 물품에 추가' 버튼을 클릭 시 발주 목록 div 안의 tbody 태그에 추가되도록 구현한 코드입니다. 수량을 선택하지 않거나 이미 추가된 물품을 선택하는 등의 예외 흐름에 대해 
조건을 따져 코드를 작성하였습니다. 물품 추가에 성공할 경우 금액을 따져 발주 물품 목록의 총 금액에 더해지도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_13.png"> <br>
-> 발주 신청자의 정보가 필요하기 때문에 로그인한 사용자의 정보를 Authentication를 사용하여 변수에 저장하였습니다. 또한 view에서 전달 받은 신청 물품 번호, 수량을 이용하기 위해
WebRequest를 사용하여 정수형 배열에 저장 후 service 메소드에 전달하도록 구현하였습니다. 

##### [Service]
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_14.png"> <br>
-> 가맹점 발주 신청 시 기존 발주 내역의 주문 번호와 겹치지 않게 하기 위해 발주 내역 목록을 전체 조회하였습니다. 주문 번호의 앞 4자리는 랜덤으로 지정하였으며 기존 발주 내역 목록 중 
주문 번호가 겹칠 경우 다시 생성하도록 구현하였습니다.

<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_15.png"> <br>
-> 주문 번호의 뒷 4자리는 발주 신청 당일로 생성되도록 구현하였습니다. 신청 일자, 신청자, 주문 번호 정보를 setFranchiseOrder 메소드로 전달하여 발주 내역 데이터를 저장하도록 구현하였습니다.
또한 setFranchiseOrderItem 메소드를 사용하여 발주 신청 물품 목록을 List 변수에 저장하고 반복문을 돌려 데이터를 저장하도록 구현하였고 변경 이력 또한 초기 정보를 이용하여 
저장하도록 구현하였습니다.

<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_16.png">
<img src="../../../../img/jaegojaego/franchiseOrderRegist/franchise-order-regist_17.png"> <br>
-> 발주 내역 정보를 셋팅하는 setFranchiseOrder 메소드와 발주 신청 물품의 정보를 셋팅하는 setFranchiseOrderItem 메소드입니다. 각 매개변수로 전달 받은 정보를 Entity의 setter를 
사용하여 저장 후 셋팅된 Entity를 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



