---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 프로젝트 상세 조회

<p style="font-weight:bold">선택한 프로젝트의 <font style="color: red;">상세 내용(제목, 시작, 종료 날짜, 설명)을 조회</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/selectProjectDetail/project-detail_1.PNG"> <br>

! 제가 클라이언트라면 [프로젝트 목록 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/selectProjectDetail/project-detail_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 프로젝트를 상세 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/byat/selectProjectDetail/project-detail_3.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다. <br>
! 로그인한 사용자가 포함된 프로젝트 목록, 프로젝트 구성원 목록을 조회하고 각 프로젝트의 시작, 종료 일자를 현재 날짜와 비교하여 진행 상태를 변경하여 보여지도록 구성하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 프로젝트 상세 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 프로젝트 상세 조회 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/selectProjectDetail/project-detail_4.PNG"> <br>
-> 프로젝트 목록 페이지에서 상세 조회할 프로젝트 우측의 설정 버튼을 클릭 후 나오는 조회/수정 버튼을 클릭합니다. <br>

<img src="../../../../img/byat/selectProjectDetail/project-detail_5.PNG"> <br>
-> 클릭 시 상세 조회 모달창이 조회되며 제목, 시작, 종료 날짜, 설명을 조회할 수 있습니다.

#### 코드

##### [Controller]
<img src="../../../../img/byat/selectProjectDetail/project-detail_6.PNG"> <br>
-> 비동기 방식으로 진행하였기 때문에 response를 사용하여 contentType을 설정하였습니다. 그 후 view에서 전달 받은 프로젝트 코드를 사용하여 service 메소드에 전달하고 프로젝트 상세 정보를 반환받도록 구현하였습니다. 
그 후 ObjectMapper를 사용하여 날짜 포맷을 설정하고 프로젝트 상세 정보를 ModelAndView 변수에 저장한다음 view에 전달하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/selectProjectDetail/project-detail_7.PNG"> <br>
-> controller에서 전달 받은 프로젝트 코드를 사용하여 프로젝트 상세 정보와 프로젝트 구성원 목록을 조회합니다. 그 후 프로젝트 구성원 중 PM인 멤버를 찾아 반환할 프로젝트 상세 정보에 저장 후 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



