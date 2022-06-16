---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 본사 발주 내역 수정

<p style="font-weight:bold">본사가 발주 내역의 처리 상태가 <font style="color:red;">"처리 전"</font>상태인 발주 내역을 수정할 수 있습니다. 
  <font style="color:red;">기존 물품의 수량을 수정하거나 새로운 물품을 추가</font>하여 발주를 수정할 수 있습니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_1.png"> <br>

! 제가 클라이언트라면 [본사 발주 내역 수정] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 발주 내역을 수정할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_3.png">
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_4.png"> <br>

! 본사 발주 내역 수정은 기본적으로 발주 신청과 같은 작업이 이루어지지만 발주 신청 물품 데이터, 발주 신청서 데이터, 발주 신청서 물품 데이터를 모두 지운 후 다시 작업하도록 구현하였습니다.
 1. Controller 에서 전달 받은 수정할 발주 내역 번호 정보를 토대로 기존 발주 신청 물품 데이터, 발주 신청서 데이터, 발주 신청서 물품 데이터를 불러옵니다.
 2. 본사 발주 물품, 발주 신청서, 발주 신청서 물품 테이블의 데이터를 모두 지웁니다.
 3. 이 후, Controller에서 전달받은 정보를 이용하여 발주 신청 물품, 발주 신청서를 쿼리 메소드 save를 사용하여 저장합니다.
 4. 저장된 발주 신청서에 따라 물품이 다르게 저장되야 하기 때문에 발주 신청서 목록을 조회 후 데이터를 넣고 발주 신청서 물품들을 저장하도록 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_5.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_6.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_7.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_8.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_9.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 발주 수정 시 필요한 신청 발주 내역 번호, 물품 수량, 거래처 번호, 거래처 계약 물품 번호, 자재 번호, 신청자 번호를 제공합니다.
 2. when에서 제공받은 발주 내역 번호를 토대로 기존 발주 신청 물품, 발주 신청서, 발주 신청서 물품 테이블의 데이터를 삭제합니다.
 3. Controller에서 받은 물품 수량, 거래처 번호, 거래처 계약 물품 번호, 자재 번호, 신청자 번호로 발주 신청 물품, 발주 신청서, 발주 신청서 물품 테이블에 데이터를 save 합니다.
 4. 그 후 then에서 assertTrue를 통해 발주 수정 데이터가 제대로 저장되었는지 확인하도록 구현하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 본사 발주 내역 수정에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 본사 발주 내역 수정 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_10.png"> <br>
-> 본사 발주 내역 목록에서 처리 상태가 '처리 전' 상태인 발주 내역 중 수정할 발주 내역을 클릭합니다. 클릭 시 상세 조회 모달창이 조회되며 수정 버튼을 확인할 수 있습니다. 확인 버튼을 클릭하여 수정 페이지로 이동할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_11.png"> <br>
-> 기존 물품의 수량을 변경할 수 있으며 신청과 마찬가지로 검색하여 물품을 추가할 수 있습니다. 기존 물품 또한 취소할 수 있습니다. 우측 하단 취소 버튼을 클릭 시 발주 수정이 취소됩니다. <br>

<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_12.png"> <br>
-> 우측 하단 수정 버튼을 클릭 시 발주 수정 확인 모달창이 나오며 확인 버튼을 클릭하여 발주 수정을 완료할 수 있습니다. <br>

#### 코드

##### [Html]
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_13.png"> <br>
-> 기존 물품의 정보를 토대로 화면에 표시되며 수량을 수정할 수 있도록 thymeleaf를 사용하여 화면에 표시하도록 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_14.png"> <br>
-> 로그인한 사용자의 정보를 사용하기 위해 Authentication 의 getPincipal 메소드를 사용하여 CustomUser 자료형 변수에 저장하였습니다. WebRequest를 사용하여 view에서 전달 받은
정보를 변수에 저장하고 Sevice의 updateCompanyOrderHistory 메소드의 매개변수로 전달하여 발주 수정 작업을 진행하도록 구현하였습니다.<br>

##### [Service]
<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_15.png"> <br>
-> Controller에서 전달 받은 발주 내역 번호를 토대로 기존 발주 신청 물품, 발주 신청서, 발주 신청서 물품을 조회해와 쿼리 메소드 delete를 사용하여 데이터를 지우도록 구현하였습니다. <br>

<img src="../../../../img/jaegojaego/companyOrderModify/company-order-modify_16.png"> <br>
-> 기존 발주 신청과 같은 작업으로 수정이 이루어지도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



