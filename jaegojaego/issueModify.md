---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 이슈 제기

<p style="font-weight:bold">가맹점이 신청한 이슈를 <font style="color: red;">수정</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/issueModify/issue-modify_1.png"> <br>

! 제가 클라이언트라면 [가맹점 이슈 수정] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/issueModify/issue-modify_2.png"> <br>

! [요구 사항 명세] 를 토대로 가맹점이 이슈를 수정 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/jaegojaego/issueModify/issue-modify_3.png"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 기존 이슈의 정보를 토대로 조회한 후 수정하도록 작성하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 가맹점 이슈 수정에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 이슈 수정 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/issueModify/issue-modify_4.png"> <br>
-> 이슈 목록에서 처리 상태가 '처리 전'인 수정할 이슈를 클릭합니다. <br>

<img src="../../../../img/jaegojaego/issueModify/issue-modify_5.png"> <br>
-> 클릭 시 이슈 상세 모달창이 나옵니다. 수정 버튼을 클릭 시 수정 모달창이 조회됩니다.<br>

<img src="../../../../img/jaegojaego/issueModify/issue-modify_6.png"> <br>
-> 수정 모달창에서 제목, 내용, 첨부 파일을 수정할 수 있으며, 발주 내역과 물품은 다시 선택해야 합니다. 수정 버튼 클릭 시 확인 모달창이 조회되며 Ok버튼을 클릭 시 수정이 완료됩니다. <br>

#### 코드

##### [Html & JavaScript]
<img src="../../../../img/jaegojaego/issueModify/issue-modify_7.png"> <br>
<img src="../../../../img/jaegojaego/issueModify/issue-modify_8.png"> <br>
-> ajax를 통하여 수정 부분을 불러와 태그안에 담도록 구현하였습니다. 값을 모두 수정한 후 수정 버튼을 클릭 시 post방식으로 Controller에 요청하도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/jaegojaego/issueModify/issue-modify_9.png"> <br>
-> 수정도 이슈 제기와 마찬가지로 여러 개의 파일을 받기 위해 MultiHttpServletRequest를 사용하였습니다. 다른 점은 이미지 초기화 버튼을 눌렀는 지의 여부를 확인하도록 구현하였습니다. View 에서 받은 이슈 제목, 이슈 내용, 이슈 발생 발주 내역, 이슈 발생 물품 번호, 수량 정보를 Service단에 넘기기 위해 IssueDTO에 담아 전달합니다. <br>

<img src="../../../../img/jaegojaego/issueModify/issue-modify_10.png"> <br>
-> 파일을 생성하기 위해 파일 저장 경로, 원본 이름, 저장할 이름(UUID 사용) 등을 지정하고 파일을 생성하고 데이터 베이스에 저장하기 위해 issueAttachmentDTO에 세팅합니다. <br>

##### [Service]
<img src="../../../../img/jaegojaego/issueModify/issue-modify_11.png"> <br>
-> Controller 에서 전달받은 로그인한 사용자의 정보(customUser), 이슈 물품 정보(issueItemList), 이슈 정보(issue), 이슈 첨부파일(issueAttachmentFileList) DTO 들을 modelMapper를 통해 Entity 자료형으로 변환합니다. <br>

<img src="../../../../img/jaegojaego/issueModify/issue-modify_12.png"> <br>
-> 변환된 Entity를 토대로 이슈를 수정하고, 수정한 이슈 번호를 동적쿼리를 통해 조회합니다. <br>
조회한 이슈 번호를 사용하여 이슈 물품, 이슈 변동 내역 Entity를 삭제 후 다시 쿼리 메소드(save)를 통해 이슈 물품 정보, 이슈 변동 내역 정보들을 데이터 베이스에 저장합니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



