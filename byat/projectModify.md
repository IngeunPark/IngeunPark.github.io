---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 프로젝트 수정

<p style="font-weight:bold">권한이 PM인 멤버가 <font style="color: red;">프로젝트를 수정</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/projectModify/project-modify_1.PNG"> <br>

! 제가 클라이언트라면 [프로젝트 수정] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/projectModify/project-modify_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 프로젝트를 수정할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/byat/projectModify/project-modify_3.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다.
! 컨트롤러에서 전달 받은 내용으로 프로젝트를 수정하고, 히스토리, 변경 이력들을 저장하며 로그인한 사용자를 수정자로 저장하도록 하였고 알림이 생성되고 일정도 수정되도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/projectModify/project-modify_4.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 프로젝트 수정 요청 시 프로젝트 수정이 제대로 완료되는지 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 프로젝트 수정에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 프로젝트 수정 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/projectModify/project-modify_5.PNG"> <br>
-> 프로젝트 목록에서 수정할 프로젝트의 우측 설정 버튼을 클릭시 수정 버튼을 조회할 수 있습니다. 수정 버튼을 클릭하여 수정 모달창을 조회할 수 있습니다. <br>

<img src="../../../../img/byat/projectModify/project-modify_6.PNG"> <br>
-> 수정 모달창에서 제목, 날짜, 설명을 수정할 수 있습니다. 날짜의 경우 수정일 기준으로 수정일 뒤로 시작날짜를 미루거나 하는 작업은 수행할 수 없습니다. Ok버튼을 클릭하여 수정을 완료할 수 있습니다.

#### 코드

##### [Controller]
<img src="../../../../img/byat/projectModify/project-modify_7.PNG"> <br>
-> ModelAttribute를 사용하여 프로젝트 수정 내용을 view에서 전달 받은 정보와 session을 통한 로그인한 사용자의 정보를 사용하여 service 메소드에 전달하여 프로젝트를 service에서 수정하도록 구현하였습니다. RedirectAttritbute를 사용하여 생성 성공시 alert 창으로 생성 성공 알림을 전달하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/projectRegist/project-regist_8.PNG">
<img src="../../../../img/byat/projectRegist/project-regist_9.PNG"><br>
-> controller에 전달 받은 정보를 토대로 프로젝트를 수정하고, 로그인한 사용자의 정보를 토대로 수정자로 데이터를 저장하빈다. 또한 수정 작업이 일어났기 때문에 알림과 일정을 사용자 데이터를 이용하여 생성합니다. 히스토리와 상태 이력 데이터 또한 변경 내용으로 저장 하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



