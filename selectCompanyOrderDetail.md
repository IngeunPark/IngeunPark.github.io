---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 본사 발주 내역 상세 조회

<p style="font-weight:bold">본사가 신청한 <font style="color: red;">발주 내역</font>의 상세 내용을 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_1.png"> <br>

! 제가 클라이언트라면 [본사 발주 내역 상세 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 발주 내역을 상세 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_3.png"> <br>

? 기본적으로 본사 발주는 각 거래처 별 계약 물품에 대해 발주를 신청할 수 있는 기능입니다. <br>
? 하지만 초기 논리 모델링에서는 거래처 별 물품을 고려하지 않고 같은 물품으로 취급하고 설계를 하여 문제가 존재하였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-list_4.png"> <br>

! 최종적으로는 거래처 별 계약 물품으로 연관되도록 설계하였고 거래처 별 신청서 테이블 또한 설계하여 완성도를 높였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_3.png"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 전달 받은 값으로 쿼리 메소드 findById를 사용하여 Primary Key로 CompanyOrderHistory Entity를 반환하도록 하였습니다. 

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_4.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_5.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_6.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_7.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다. 비동기 기능이기 때문에 contentType에 APPLICATION_JSON을 사용하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 선택한 발주의 식별자 번호를 given에서 제공합니다.
 2. when에서 쿼리 메소드와 식별자를 이용하여 조회해 온 본사 발주 내역을 CompanyOrderHistory Entity 변수에 저장합니다.
 3. 그 후 then에서 assertNotNull을 통해 본사 발주 내역이 제대로 불러왔는지 확인하여 테스트를 마무리하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 본사 발주 내역 상세 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 본사 발주 내역 상세 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_8.png"> <br>
-> 본사 발주 내역 목록 조회페이지에서 상세 조회하고자 하는 발주 내역을 클릭합니다. 클릭 시 상세 조회 모달창이 화면에 출력됩니다. 신청 물품 이름, 신청 물품 수량, 물품 당 총 금액을 확인할 수 있습니다. <br>

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_9.png"> <br>
-> 화면의 이동이 없이 모달창으로 조회해야 하기 때문에 비동기 방식인 ajax를 사용하였습니다. 선택한 발주 내역의 발주 번호를 Controller에 전달하여 상세 내용을 조회할 수 있도록 구현하였습니다. 성공 시 Controller에서 전달 받은 값인 물품 이름, 수량, 금액을 반복문을 사용하여 화면에 표시되도록 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_10.png"> <br>
-> view에서 전달 받은 발주 내역 번호를 RequerstParam을 통해 CompanyOrderHistoryNo 변수에 담고 Service의 메소드를 통해 본사 발주 내역 DTO를 조회했습니다. 이 때, 본사가 발주 신청한 물품에 대해 같은 물품이여도 다른 거래처에 발주를 신청할 수 있기 때문에 비교 후 수량과 금액을 계산해야 하기 때문에 Map 자료형 변수 equalItem을 선언하고 물품 번호를 key값으로 저장하였습니다. <br>

<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_11.png"> <br>
-> 본사가 신청한 물품과 거래처 별 계약 상품과 비교 후 같을 경우 Map 변수의 key 값을 통해 금액을 누적하여 저장하였습니다. 또한 물품들의 정보를 view에 전달해야 하기 때문에 CompanyOrderDetailDTO 자료형 List에 정보들을 담고 반환하도록 구현하였습니다. <br>

##### [Service]
<img src="../../../../img/jaegojaego/companyOrderList/company-order-detail_12.png"> <br>
-> Controller에서 전달 받은 본사 발주 내역 번호(companyOrderHistoryNo)가 CompanyOrderHistory Entity의 Id, 즉 식별자 이기 때문에 companyOrderHistoryNo을 쿼리 메소드인 findById를 사용하여 본사 발주 내역을 조회하도록 구현하였습니다. <br>

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



