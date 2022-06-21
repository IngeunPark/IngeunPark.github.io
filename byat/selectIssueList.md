---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 이슈 목록 조회

<p style="font-weight:bold">각 스프린트에서 <font style="color: red;">제기된 이슈의 목록을 조회</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/issueList/issue-list_1.PNG"> <br>

! 제가 클라이언트라면 [이슈 목록 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/issueList/issue-list_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 이슈의 목록을 조회할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

## [자세한 내용](https://www.notion.so/64f066b6ee4948f0926f0790b553dcad)

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/byat/selectProjectList/project-list_3.png"> <br>

? 기본적으로 프로젝트와 구성원 테이블은 변경이 있을 때마다 변경 사항을 저장할 테이블이 필요합니다. <br>
? 하지만 초기 논리 모델링에서는 히스토리와 변경 이력을 저장하지 않고 단순히 기본 테이블들만 존재하여 문제가 존재하였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/byat/selectProjectList/project-list_4.PNG"> <br>

! 최종적으로는 프로젝트와 구성원 테이블의 히스토리, 변경 이력 테이블을 통하여 관리할 수 있도록 구성하였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/byat/issueList/issue-list_3.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다.
! 스프린트 목록을 먼저 조회한 후 진행 중인 스프린트의 이슈 목록을 조회하도록 하였고 이슈 구성원 또한 조회하여 컨트롤러에 반환하도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/issueList/issue-list_4.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 이슈 목록 조회 관련 요청이 제대로 수행되는지 직접 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 이슈 목록 조회 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 이슈 목록 조회 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/issueList/issue-list_5.PNG"> <br>
-> 이슈를 조회하기 위해 프로젝트를 상세 페이지로 이동합니다. 프로젝트의 제목을 클릭하여 상세 페이지로 이동할 수 있습니다.

<img src="../../../../img/byat/issueList/issue-list_6.PNG"> <br>
-> 프로젝트 상세 페이지 좌측 메뉴 버튼을 클릭 후 Issue 버튼을 클릭하여 이슈 페이지로 이동합니다.

<img src="../../../../img/byat/issueList/issue-list_7.PNG"> <br>
-> 이슈 목록 페이지입니다. 이슈 제목, 제기자, 제기 일자, 상태를 확인할 수 있습니다.

#### 코드

##### [Controller]
<img src="../../../../img/byat/issueList/issue-list_8.PNG"> <br>
-> view에서 전달 받은 프로젝트 코드를 service 메소드에 전달하여 이슈 목록이 담긴 SpringDTO List를 반환받도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/issueList/issue-list_9.PNG">
<img src="../../../../img/byat/issueList/issue-list_10.PNG"> <br>
-> service와 이슈 목록 쿼리문입니다. Controller에서 전달 받은 프로젝트 코드를 사용하여 스프린트 목록을 조회합니다. 그 후 스프린트 목록의 이슈 목록을 조회하고, 이슈 참여자 또한 조회하여 setter SprintDTO 에 담고 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



