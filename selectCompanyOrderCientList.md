---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 본사 발주 내역 거래처 목록 조회

<p style="font-weight:bold">본사가 발주 내역의 <font style="color:red;">발주를 신청한 거래처의 목록</font>을 조회할 수 있는 기능입니다.</p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_1.png"> <br>

! 제가 클라이언트라면 [본사 발주 내역 거래처 목록 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 본사 발주 내역 거래처 목록을 조회할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_3.png"><br>

! 본사 발주 내역 거래처 목록은 결국 발주 내역 상세 데이터에 존재하기 때문에 상세 내용을 조회한 후 Controller에서 거래처 목록으로 가공하여 DTO에 담고 view에 반환하도록 시퀀스 다이어그램을 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_4.png"> <br>
<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_5.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다. 비동기 방식으로 코드를 작성하였기 때문에 contentType 값에 APPLICATION_JSON을 사용하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 본사 발주 내역 거래처 목록 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 본사 발주 내역 거래처 목록 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_6.png"> <br>
-> 본사 발주 내역 목록 페이지에서 거래처 목록을 조회할 발주 내역의 출력 버튼을 클릭 시 거래처 목록 모달창을 조회할 수 있습니다.

<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_7.png"> <br>
-> 모달창에서 발주 신청 물품에 해당 하는 거래처들의 목록을 조회할 수 있으며 발주 신청서 상세 조회가 가능한 '출력' 버튼이 조회됩니다. 확인 버튼을 클릭 시 모달창이 닫히며 '출력' 버튼을 클릭 시 발주 신청서 상세 조회 페이지로 이동합니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_8.png">
<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_9.png">
-> '출력'버튼을 클릭 시 비동기 방식으로 동작해야 하기 때문에 ajax로 작성하였습니다. 발주 내역 번호를 전송하고 성공하면 필요한 정보를 data 변수에 담기도록 구현하였습니다.
data 변수에 저장된 거래처 이름을 사용하여 화면에 나타내도록 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/selectCompanyOrderCientList/company-order-client-list_10.png"> <br>
-> view에서 전달 받은 발주 내역 번호를 사용하기 위해 @RequestParam 어노테이션 변수를 사용하였습니다. 발주 내역 번호를 통해 service 메소드의 발주 내역 상세 조회
하여 DTO에 저장하였고 DetailDTO에 가공하여 저장 후 view페이지에 Gson 을 사용하여 반환하도록 구현하였습니다. <br>

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



