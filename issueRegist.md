---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

# 가맹점 이슈 제기

*** <p style="color: red;">가맹점이 발주 신청 물품을 실제로 받았을 때 물품에 대해 문제가 존재할 경우 이슈를 제기할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_1.png"> <br>

! 제가 클라이언트라면 [가맹점 이슈 제기] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_2.png"> <br>

! [요구 사항 명세] 를 토대로 가맹점이 이슈를 제기 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_3.png"> <br>

? 기본적으로 이슈 제기는 본사가 가맹점이 신청한 발주에 대해 출고를 하면 가맹점이 신청한 발주에 대해 물품을 받고, 그 물품이 문제가 생겼을 때 제기할 수 있습니다. <br>
? 하지만 초기 논리 모델링에서는 단순히 발주, 출고와 연관짓지 않고 단순히 이슈로만 테이블을 구성하였기에 한 눈에 봐도 문제가 많아 보였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_4.png"> <br>

! 최종적으로는 발주에 대해 출고된 발주 물품에 대해 이슈를 제기할 수 있도록 하기 위해 출고와 연관지어 테이블을 구성하였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_5.png"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 하나의 서비스 메소드에서 가맹점 이슈에 대한 정보를 설정하여 데이터베이스에 저장하고 저장한 이슈의 번호를 토대로 이슈 물품, 변경이력을 저장하도록 설계하였습니다.

#### [단위 테스트 & 테스트 코드]

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_6.png"> <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_7.png"> <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_8.png"> <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_9.png"> <br>

! 서비스 메소드 테스트를 given/when/then 형식으로 작성하였습니다. <br>
 1. 이슈 제기 시 필요한 정보들을 given에서 제공합니다.
 2. when에서 이슈 생성으로 인해 생기는 이슈 번호를 통해, 이슈 변경 이력, 이슈 발생 물품을 save하여 데이터 베이스에 저장합니다. 
 3. 그 후 then에서 생성된 이슈 번호를 통해 repository로 조회한 결과값들이 Null이 아닌지 확인하여 테스트를 마무리하였습니다.

#### Result
! [요구사항명세]로 클리이언트가 가맹점 이슈에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 가맹점 이슈 단위의  [단위업무정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리모델링]으로  [시퀀스다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 테스트 주도 개발을 위해 구현 전 테스트 코드를 작성하여 테스트 한 뒤 [단위테스트]에 결과를 작성하였습니다. 

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_10.png"> <br>
-> 이슈 목록에서 우측 상단 이슈 제기 버튼을 클릭합니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_11.png"> <br>
-> 버튼 클릭 시 이슈 제기 모달창이 나옵니다. 제목, 내용을 직접 입력할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_12.png"> <br>
-> 발주 내역 selectBox 클릭 시 해당 가맹점의 출고 완료된 발주 내역을 조회할 수 있으며 이슈가 발생한 발주 내역을 선택해야 합니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_13.png"> <br>
-> 발주 내역 선택 시 해당 발주 물품 중 이슈 발생 물품을 선택할 수 있습니다. 선택 시 하단 물품 목록칸에 물품이 추가되며 수량을 입력할 수 있습니다. 우측 상단 이미지 업로드 버튼 클릭 시 여러 개의 이미지 파일을 추가할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_14.png"> <br>
-> 이미지 업로드시 이미지 파일이 미리보기형태로 조회됩니다. 우측 하단 제기 버튼 클릭 시 이슈 확인 모달창이 조회됩니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_15.png"> <br>
-> Cancel 버튼 클릭 시 모달창으로 다시 돌아가며, Ok 버튼 클릭 시 이슈 제기에 성공합니다.

#### 코드

##### [javaScript]
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_16.png"> <br>
-> 목록 페이지에서 이슈 제기 버튼 클릭 시 비동기로 동작해야 하기에 ajax를 통해 로그인한 가맹점 계정과 관련된 사용자들이 제기한 발주 목록 중 출고 완료된 발주 목록을 조회하여 option 태그를 동적생성합니다. 그 결과, 가맹점이 이슈 제기 시 selectBox를 통해 문제가 발생한 발주 내역을 선택할 수 있습니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_17.png"> <br>
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_18.png"> <br>
-> 이미지 업로드 버튼을 클릭하여 이미지를 추가 시 이미지 목록들을 불러와 fileReader로 이미지 파일을 읽어 img 태그를 동적생성합니다. 여러 파일을 업로드 할 수 있기 때문에 list 형태로 변수를 저장하였고 업로드 한 파일을 화면에서 미리보기 할 수 있도록 imageUplodaFunction을 구현하였습니다. <br>

##### [Controller]
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_19.png"> <br>
-> 여러 개의 파일을 받기 위해 MultiHttpServletRequest를 사용하였습니다. View 에서 받은 이슈 제목, 이슈 내용, 이슈 발생 발주 내역, 이슈 발생 물품 번호, 수량 정보를 Service단에 넘기기 위해 IssueDTO에 담아 전달합니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_20.png"> <br>
-> 파일을 생성하기 위해 파일 저장 경로, 원본 이름, 저장할 이름(UUID 사용) 등을 지정하고 파일을 생성하고 데이터 베이스에 저장하기 위해 issueAttachmentDTO에 세팅합니다. <br>

##### [Service]
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_21.png"> <br>
-> controller 에서 전달받은 로그인한 사용자의 정보(customUser), 이슈 물품 정보(issueItemList), 이슈 정보(issue), 이슈 첨부파일(issueAttachmentFileList) DTO 들을 modelMapper를 통해 Entity 자료형으로 변환합니다. <br>

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_22.png"> <br>
-> 변환된 Entity를 토대로 이슈를 생성하고, 생성한 이슈 번호를 동적쿼리를 통해 조회합니다. <br>
조회한 이슈 번호를 사용하여 이슈 물품, 이슈 변동 내역 Entity에 이슈 번호를 저장하고 쿼리 메소드(save)를 통해 이슈 물품 정보, 이슈 변동 내역 정보들을 데이터 베이스에 저장합니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#first)



