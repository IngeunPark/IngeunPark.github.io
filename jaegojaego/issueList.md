---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 이슈 목록 조회

<p style="font-weight:bold">가맹점이 제기한 <font style="color: red;">이슈 목록</font>을 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/issueList/issue-list_1.png"> <br>

! 제가 클라이언트라면 [가맹점 이슈 목록 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/issueList/issue-list_2.png"> <br>

! [요구 사항 명세] 를 토대로 가맹점이 이슈의 목록을 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

## [자세한 내용](https://www.notion.so/912b85f8f7f645b6859401cccae0124b)

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_3.png"> <br>

? 기본적으로 이슈 제기는 본사가 가맹점이 신청한 발주에 대해 출고를 하면 가맹점이 신청한 발주에 대해 물품을 받고, 그 물품이 문제가 생겼을 때 제기할 수 있습니다. <br>
? 하지만 초기 논리 모델링에서는 단순히 발주, 출고와 연관짓지 않고 단순히 이슈로만 테이블을 구성하였기에 한 눈에 봐도 문제가 많아 보였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_4.png"> <br>

! 최종적으로는 발주에 대해 출고된 발주 물품에 대해 이슈를 제기할 수 있도록 하기 위해 출고와 연관지어 테이블을 구성하였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/jaegojaego/issueList/issue-list_3.png">
<img src="../../../../img/jaegojaego/issueList/issue-list_4.png">
<img src="../../../../img/jaegojaego/issueList/issue-list_5.png"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 이슈 목록을 조회할 때 3가지 경우의 수가 존재하며 그에 맞게 시퀀스 다이어그램을 각각 작성하였습니다.
 1. 본사의 경우 담당 중인 가맹점 계정(대표, 매니저)가 제기한 이슈의 목록을 조회할 수 있습니다.
 2. 가맹점 대표의 경우 대표가 제시한 이슈와 매니저가 제기한 이슈의 목록을 조회할 수 있습니다.
 3. 가맹점 매니저의 경우 대표가 제시한 이슈와 매니저가 제기한 이슈의 목록을 조회할 수 있습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/issueList/issue-list_6.png"> <br>
<img src="../../../../img/jaegojaego/issueList/issue-list_7.png"> <br>
<img src="../../../../img/jaegojaego/issueList/issue-list_8.png"> <br>
<img src="../../../../img/jaegojaego/issueList/issue-list_9.png"> <br>
<img src="../../../../img/jaegojaego/issueList/issue-list_10.png"> <br>

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 이슈 목록 조회 시 필요한 로그인 한 사용자가 본사 계정인지 가맹점 대표 또는 매니저인지와 본사 회원 번호를 given에서 제공합니다.
 2. when에서 경우의 수 에 따라 가맹점 대표 정보와 매니저 정보를 조회한 후 따로 선언한 메소드를 통해 필요한 값을 IssueDetailDTO 자료형의 List에 저장합니다.
 3. 그 후 then에서 제대로 조회되어 저장되었는지 assertNotNull을 사용하여 확인하도록 작성하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 이슈 목록 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 이슈 목록 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/issueList/issue-list_11.png"> <br>
-> 가맹점 발주 관리 메뉴의 하위 메뉴인 가맹점 이슈 메뉴를 클릭 시 이슈 목록 페이지로 이동할 수 있습니다. 이슈 제기 가맹점명, 이슈 제목, 이슈 제기 일자, 이슈 제기자, 처리 일자
, 처리자, 처리 상태를 조회할 수 있습니다. <br>

#### 코드

##### [Html]
<img src="../../../../img/jaegojaego/issueList/issue-list_12.png"> <br>
-> Controller에서 전달 받은 이슈 목록을 나타낼 정보들을 thymeleaf를 사용하여 화면에 표시되도록 구현하였습니다. if문을 사용하여 제기자가 대표인지 매니저인지 확인하도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/issueList/issue-list_13.png"> <br>
-> 로그인한 사용자 정보를 service 메소드에 전달하여 IssueDetailDTO 자료형 List를 반환받습니다. 그 후 view에 이슈 목록과 로그인한 사용자 정보를 전달하도록 구현하였습니다.

##### [Service]
<img src="../../../../img/jaegojaego/issueList/issue-list_14.png">
<img src="../../../../img/jaegojaego/issueList/issue-list_15.png">
<img src="../../../../img/jaegojaego/issueList/issue-list_16.png">
<p style="font-weight:bold;">가맹점 계정의 경우의 메소드 setIssueDetailByFranchise </p>
<img src="../../../../img/jaegojaego/issueList/issue-list_17.png">
<p style="font-weight:bold;">본사 계정의 경우 setIssueDetail </p>
<img src="../../../../img/jaegojaego/issueList/issue-list_18.png">
<img src="../../../../img/jaegojaego/issueList/issue-list_19.png"><br>
-> Controller에서 전달 받은 사용자의 정보를 토대로 가맹점의 정보를 가져옵니다. 본사 계정의 경우 setIssueDetail 메소드를 통해 가맹점 대표와 가맹점 매니저별로 제기한 이슈의 목록을 저장 후 합쳐 반환하고, 가맹점 계정의 경우 setIssueDetailByFranchise 메소드를 사용하여 매개변수로 받은 정보들을 하나로 합친 후 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



