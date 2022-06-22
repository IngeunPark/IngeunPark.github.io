---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 이슈 상세 조회

<p style="font-weight:bold">가맹점이 제기한 <font style="color: red;">이슈의 상세 내용</font>을 조회할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_1.png"> <br>

! 제가 클라이언트라면 [가맹점 이슈 상세 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/issueDetail/issue-detail_2.png"> <br>

! [요구 사항 명세] 를 토대로 본사 계정과 가맹점 계정이 이슈를 상세 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/issueDetail/issue-detail_3.png">

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 이슈 번호를 토대로 쿼리 메소드 findById를 사용하여 이슈를 상세 조회하고 해당 이슈의 첨부 파일 목록 또한 
조회할 수 있도록 작성하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/issueDetail/issue-detail_4.png"> <br>
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_5.png">
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_6.png"> <br>
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_7.png"> <br>

! 컨트롤러 테스트를 MockMVC 를 통해 배포 전 스프링 MVC 의 동작을 테스트하였습니다. 비동기 기능이기 때문에 contentType에 APPLICATION_JSON을 사용하였습니다.

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다.
 1. 이슈 상세 조회 시 필요한 이슈 번호를 given에서 제공합니다.
 2. when에서 쿼리 메소드를 사용하여 이슈 상세 정보와 이슈에 해당하는 삭제되지 않은 첨부파일 목록을 조회하고 IssueDTO 에 setter를 사용하여 저장합니다.
 3. 그 후 then에서 IssueDTO가 NotNull인지 확인하도록 구한하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 이슈 상세 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 이슈 상세 조회 단위의  [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위 테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_8.png"> <br>
-> 이슈 목록 페이지에서 상세 조회할 이슈를 클릭합니다. <br>

<img src="../../../../img/jaegojaego/issueDetail/issue-detail_9.png"> <br>
-> 클릭 시 이슈 상세 조회 모달창이 조회되며 제목, 내용, 이슈 물품(이름, 수량) 목록, 이미지 첨부파일 목록을 조회할 수 있습니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_10.png"> <br>
-> 비동기 방식 ajax를 사용하여 상세 조회를 하도록 구현하였습니다. 성공 시 미리 선언한 태그에 값을 넣어 화면에 표시되도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_11.png"> <br>
-> view에서 전달 받은 이슈 번호를 WebRequest를 사용하여 변수에 저장하고 service 메소드에 전달하여 이슈 상세 정보를 반환받도록 구현하였습니다. Gson을 사용하여 view에 전달하도록 구현하였습니다.

##### [Service]
<img src="../../../../img/jaegojaego/issueDetail/issue-detail_12.png"> <br>
-> Controller에서 전달 받은 이슈 번호로 쿼리 메소드를 사용하여 이슈 상세 내용을 조회합니다. 또한 이슈 번호로 이슈에 해당하는 첨부파일을 쿼리 메소드를 사용하여 조회 후 이슈 상세 DTO에 setter를 사용하여 저장합니다. 그 후 이슈 상세 내용을 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



