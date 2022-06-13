---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 본사 발주 내역 발주 신청서 상세 조회

<p style="font-weight:bold">본사가 발주 <font style="color:red;">신청할 거래처 별</font>로 전달할 <font style="color:red;">발주 신청서</font>를 상세 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_1.png"> <br>

! 제가 클라이언트라면 [본사 발주 내역 수정] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 발주 내역의 발주 신청서를 상세 조회할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_3.png"><br>

! 본사 발주 내역 발주 신청서 상세 조회는 발주 신청 시 선택한 거래처들 중 선택한 하나의 거래처의 발주 신청서를 상세 조회할 수 있습니다.
! 발주 신청자와 본사의 정보, 거래처의 정보, 배송지, 발주 신청 물품, 수량 등을 확인할 수 있으며 발주 신청서에서는 신청 물품 이름이 거래처 기준으로 조회됩니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_4.png">
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_5.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_6.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_7.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_8.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다. 발주 내역 번호와 거래처 번호를 parameter로 제공하여 발주 내역에서 선택한 거래처의 발주 신청서를 조회할 수 있도록 하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 발주 신청서 상세 조회시 발주 내역을 선택 후 거래처를 선택하기 때문에 발주 내역 번호와 거래처 번호를 제공하고 발주 신청서를 저장할 OrderApplication 자료형의 List를 제공하였습니다.
 2. when에서 제공받은 발주 내역 번호로 발주 내역을 조회한 후 CompanyOrderHisory Entity 자료형 변수에 담고 발주 내역의 거래처 번호와 비교하여 같을 경우 발주 신청서 List에 저장하도록 구현하였습니다.
 3. 그 후 then에서 assertNotNull을 통해 해당 거래처의 발주 신청서가 제대로 조회되고 저장되었는지 확인합니다.

#### Result
! [요구 사항 명세]로 클리이언트가 본사 발주 내역 거래처 발주 신청서 상세 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 본사 발주 내역 거래처 발주 신청서 상세 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_9.png"> <br>
-> 발주 내역 거래처 목록에서 발주 신청서 상세 조회할 거래처의 '출력'버튼을 클릭하여 발주 신청서 상세 조회 페이지로 이동할 수 있습니다.

<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_10.png"> <br>
-> 해당 거래처 정보, 본사 발주 신청자 정보, 배송지 정보, 신청 물품 정보를 조회할 수 있습니다. 또한 우측 상단 download 버튼을 클릭 시 해당 발주 신청서가 PDF파일 형식으로 다운로드 됩니다.

#### 코드

##### [Html]
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_11.png"> <br>
-> Controller에서 전달 받은 발주 신청서의 정보를 토대로 thymeleaf를 사용하여 화면에 조회되도록 구현하였습니다. <br>

##### [JavaScript]
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_12.png"> <br>
-> PDF 파일로 다운로드하기 위한 코드입니다. 저장을 위한 savaAs 함수와 PDF파일 형태로 구성하기 위한 html2canvas API를 사용하였습니다. html2canvas 에서 크기와 형태를 잡고
버튼 클릭 시 다운로드 되도록 하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_13.png"> <br>
-> WebRequest를 사용하여 view에서 전달한 발주 내역 번호와 거래처 번호를 변수에 담고 service 메소드에 전달하였습니다. 그리고 반환 받은 발주 신청서의 정보를 토대로 발주 내역 정보,
 발주 신청서 정보, 발주 신청서 물품 정보를 가공하여 ModelAndView를 사용하여 view에 전달하도록 구현하였습니다. <br>

##### [Service]
<img src="../../../../img/jaegojaego/companyOrderApplication/company-order-orderApplication_14.png"> <br>
-> Controller에서 전달 받은 발주 내역 번호를 사용하여 발주 신청서 목록을 조회합니다. 발주 신청서 목록 중 전달 받은 거래처 번호와 같은 발주 신청서를 변수에 저장 후 ModelMapper를
 사용하여 Entity를 DTO 형식으로 변환하여 Controller에 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



