---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 아슈 처리 상태 변경

<p style="font-weight:bold">가맹점이 제기한 이슈에 대해 <font style="color: red;">본사가 처리 상태를 변경</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_1.png"> <br>

! 제가 클라이언트라면 [가맹점 이슈 처리 상태 변경] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사가 가맹점 이슈의 처리 상태를 변경할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_3.png"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였습니다. <br>
! 쿼리 메소드 findById를 사용하여 처리 상태를 변경할 이슈를 조회한 후 변경자 정보를 조회하여 setter를 사용하여 처리 상태를 변경하고 변경 이력 또한 save 메소드를 사용하여 저장하도록 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_4.png"> <br>
<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_5.png"> <br>

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 가맹점 이슈 처리 상태 변경을 위해 given에서 변경할 이슈 번호와 변경할 처리 상태를 제공합니다. 또한 변경자 정보를 제공합니다.
 2. when에서 가맹점 이슈 번호를 통해 변경할 이슈를 조회후 setter를 통해 처리 상태를 변경하였습니다.
 3. 그 후 이슈 처리 변경 이력에 필요한 정보들을 setter를 이용하여 저장 한뒤 처리 상태가 처리 완료일 경우 처리자 정보를 저장하도록 하였습니다.
 4. then에선 처리 상태가 변경되었는지 확인하기 위해 처리자 정보를 assertTrue로 비교하여 테스트하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 이슈 처리 상태 변경에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 이슈 처리 상태 변경 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_6.png"> <br>
-> 변경할 가맹점 이슈의 처리 상태를 클릭합니다. <br>

<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_7.png"> <br>
-> 버튼 클릭 시 변경 가능한 상태 버튼이 조회됩니다.

<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_8.png"> <br>
-> 처리 상태 변경 확인 모달창입니다. Ok 버튼 클릭 시 처리 상태 변경이 완료됩니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_9.png">
<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_10.png"><br>
-> 변경 버튼 클릭 시 먼저 이슈 번호와 변경할 처리 상태 정보를 변수에 저장한 뒤 flag변수 값을 변경합니다. 그 후 flag 변수의 값을 따져서 조건이 맞을 경우 fetch를 사용하여 비동기 방식으로 작업을 수행하도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_11.png"> <br>
-> view에서 전달 받은 이슈 번호와 처리 상태가 저장된 statusChangeIssueInfo 변수를 사용하기 위해 RequestBody 어노테이션을 사용하였습니다. JsonParser와 JsonElement를 사용하여
전달 받은 정보를 변수에 저장하도록 하고 service 메소드에 전달하였습니다. 

##### [Service]
<img src="../../../../img/jaegojaego/issueModifyStatus/franchise-issue-modify-status_12.png"><br>
-> Contoller에서 제공 받은 가맹점 이슈 번호로 쿼리 메소드 findById 를 사용하여 가맹점 이슈를 조회하여 Entity 변수에 저장합니다. 
그 후 setter를 사용하여 처리 상태를 변경하도록 하였고 변경 이력 Entity인 IssueHistory 또한 선언하고 setter 메소드를 통해 값을 저장하였습니다. 
변경할 처리 상태가 처리 완료일 조건을 따져 본사 처리자의 정보를 조회 후 저장하도록 하였습니다.
최종적으로 필요한 정보가 모두 저장된 이슈 변경 이력을 save 메소드를 사용하여 저장하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



