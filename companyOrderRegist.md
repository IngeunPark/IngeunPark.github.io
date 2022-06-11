---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 본사 발주 신청

<p style="font-weight:bold">본사가 발주 가능 신청 물품을 이름을 통해 <font style="color:red;">검색하고 추가</font>할 수 있으며 수량을 직접 입력하고 
  <font style="color:red;">해당 물품을 계약한 거래처를 선택</font>하여 발주를 신청할 수 있습니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_1.png"> <br>

! 제가 클라이언트라면 [본사 발주 신청] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 발주를 신청 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

## [자세한 내용](https://www.notion.so/912b85f8f7f645b6859401cccae0124b)

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_3.png"> <br>

? 기본적으로 본사 발주는 각 거래처 별 계약 물품에 대해 발주를 신청할 수 있는 기능입니다. <br>
? 하지만 초기 논리 모델링에서는 거래처 별 물품을 고려하지 않고 같은 물품으로 취급하고 설계를 하여 문제가 존재하였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_4.png"> <br>

! 최종적으로는 거래처 별 계약 물품으로 연관되도록 설계하였고 거래처 별 신청서 테이블 또한 설계하여 완성도를 높였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_3.png"> <br>

! 본사 발주 신청은 발주 신청 데이터, 발주 신청 물품 데이터, 각 거래처별로 전달할 발주 신청서에 대한 데이터, 발주 신청서 물품 데이터에 값이 저장되어야 하기 때문에 하나의 
서비스 생명주기에서 이루어지도록 구현하였습니다.
 1. Controller 에서 전달 받은 정보를 토대로 쿼리메소드 save를 사용하여 본사 발주 테이블에 데이터를 저장합니다.
 2. 본사 발주 물품, 발주 신청서, 발주 신청서 물품 테이블엔 본사 발주 내역 번호가 필요하기 때문에 repository에 Native 쿼리를 사용한 selectRecentHistoryNo 메소드를 통해 반환받습니다.
 3. 이 후, 반환받은 본사 발주 내역 번호를 이용하여 발주 신청 물품, 발주 신청서를 쿼리 메소드 save를 사용하여 저장합니다.
 4. 저장된 발주 신청서에 따라 물품이 다르게 저장되야 하기 때문에 발주 신청서 목록을 조회 후 데이터를 넣고 발주 신청서 물품들을 저장하도록 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_4.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_5.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_6.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_7.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_8.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_9.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_10.png"> <br>


! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 발주 신청시 필요한 신청 물품 수량, 거래처 번호, 거래처 계약 물품 번호, 자재 번호, 신청자 번호를 제공합니다.
 2. when에서 쿼리 메소드와 제공받은 정보를 토대로 발주 데이터 저장, 발주 물품 데이터 저장, 발주 신청서 데이터 저장, 발주 신청서 물품 데이터 저장을 하도록 작성하였습니다.
 3. 그 후 then에서 assertNotNull을 통해 본사 발주 내역, 해당 발주 내역의 물품, 해당 발주 내역의 발주 신청서, 해당 발주 신청서의 물품 정보가 제대로 저장되었는지 확인하여 테스트를 마무리하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 본사 발주 신청에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 본사 발주 내역 상세 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_11.png"> <br>
-> 본사 발주 관리의 하위 메뉴인 발주 신청을 클릭하여 발주 신청 페이지로 이동할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_12.png"> <br>
-> 발주 신청할 물품의 이름을 통해 물품을 검색할 수 있습니다. 클릭 시 발주 물품 목록에 추가됩니다. <br>

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_13.png"> <br>
-> 수량을 입력할 수 있으며 물품에 대해 계약한 거래처 중 발주를 신청할 거래처를 선택할 수 있습니다. 또한 취소(X) 버튼을 클릭 해 물품 선택을 취소할 수 있습니다.  <br>

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_14.png"> <br>
-> 우측 하단 발주 버튼을 클릭 시 발주 신청 확인 모달창이 나오며 확인 버튼을 클릭하여 발주 신청을 완료할 수 있습니다. <br>

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_15.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_16.png"> <br>
-> 물품 검색 시 자동 완성 기능을 사용하기 위해 ajax autocomplete를 사용하였습니다. 물품을 클릭 시 동적으로 td 태그를 생성하여 화면에 표시되도록 구현하였습니다. <br>

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_17.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_18.png"> <br>
-> 발주 신청 버튼 클릭 시 flag 변수를 통해 발주 신청 시 확인해야 할 예외를 확인하였고 예외가 발생하지 않을 시 발주 신청을 완료할 수 있도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_19.png"> <br>
-> view에서 전달 받은 여러 물품 수량, 물품 번호, 거래처 번호, 거래처 판매 계약 상품 번호를 각 배열에 담고 서비스 메소드의 매개 변수로 전달하도록 구현하였습니다. 발주 신청 완료 후 
거래처 별 발주 신청서를 확인할 수 있게 하기 위해 CompanyOrderDetailDTO 자료형의 List를 반환받고 view에 전달하도록 구현하였습니다. <br>

##### [Service]
<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_20.png"> <br>
-> 같은 물품을 신청하더라도 다른 거래처에서 발주를 신청한 경우가 존재하기 때문에 Map을 사용하여 수량을 관리하도록 구현하였습니다. 또한 중복된 거래처가 존재하기 때문에
중복된 거래처를 제거하기 위해 List에 담고 containsKey를 사용하여 확인하고 저장하도록 구현하였습니다.

<img src="../../../../img/jaegojaego/companyOrderRegist/company-order-regist_21.png"> <br>
-> 또한 저장해야 될 테이블 각각 마다 메소드를 작성하여 유지보수 관리에 용이하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



