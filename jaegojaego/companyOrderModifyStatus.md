---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 본사 발주 처리 상태 변경

<p style="font-weight:bold">본사가 발주 내역의 처리 상태가 <font style="color:red;">"처리 전"</font>상태인 발주 내역의 처리 상태를 변경할 수 있습니다. 
  <font style="color:blue;">승인완료</font>또는 <font style="color:red;">신청취소</font>로 발주의 처리 상태를 변경할 수 있습니다.</p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_1.png"> <br>

! 제가 클라이언트라면 [본사 발주 내역 처리 상태 변경] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 발주 처리 상태를 변경할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_3.png"> <br>

! 본사 발주 내역 처리 상태 변경은 발주 내역 번호를 토대로 service에서 쿼리 메소드 findById를 사용하여 발주 내역을 불러 온 후 전달 받은 처리 상태로 변경하도록 시퀀스 다이어그램을 작성하였습니다. 

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_4.png"> <br>
<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_5.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 발주 처리 상태 변경 시 필요한 수정자 회원 번호, 상태 변경할 발주 내역 번호, 변경할 처리 상태를 제공합니다.
 2. when에서 제공 받은 발주 내역 번호으로 변경할 발주 내역을 조회하여 Entity 자료형에 저장한 후 setter 메소드를 사용하여 처리 상태와 수정자 데이터를 변경합니다.
 3. then에선 제공 받은 처리 상태와 변경 후의 발주 내역의 처리 상태와 비교하여 값이 같을 경우를 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 본사 발주 내역 처리 상태 변경에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 본사 발주 내역 처리 상태 변경 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_6.png"> <br>
-> 본사 발주 내역 목록 페이지에서 처리 상태가 '처리 전'인 발주 내역 중 처리 상태를 변경할 발주 내역의 처리 상태를 클릭하여 변경할 처리 상태 버튼 모음을 조회할 수 있습니다.

<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_7.png"> <br>
-> 클릭 시 '승인 완료'로 처리 상태를 변경할 수 있는 버튼과 '신청 취소'로 처리 상태를 변경할 수 있는 버튼이 조회됩니다. 둘 중 하나를 선택해서 처리 상태를 변경할 수 있습니다. 

<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_8.png"> <br>
-> 버튼 중 하나를 클릭 시 변경 확인 모달창이 나오며 확인 버튼을 클릭하여 처리 상태를 변경할 수 있습니다. <br>

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_9.png">
<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_10.png"> <br>
-> sweetAlert창을 통하여 변경을 확인 받고 변경 요청이 있을 경우 비동기 방식 fetch를 통하여 Controller에 요청하고 요청이 성공하면 flag변수에 성공 값을 저장하도록 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_11.png"> <br>
-> 로그인 한 사용자 정보와 view에서 전달 받은 처리 상태 변경 값과 발주 내역 번호를 사용하여 service 메소드에 전달하도록 구현하였습니다. 처리 상태 변경이 성공적으로 이루어질 경우
view에 성공 메세지를 반환하도록 구현하였습니다.

##### [Service]
<img src="../../../../img/jaegojaego/companyOrderModifyStatus/company-order-modify-status_12.png"> <br>
-> Controller에서 전달 받은 발주 내역 번호를 토대로 쿼리 메소드 findById를 사용하여 발주 내역을 조회한 후 Entity에 저장합니다. 그 후 setter 메소드를 사용하여
처리 상태, 변경 일자, 수정자 정보를 변경하여 저장하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



