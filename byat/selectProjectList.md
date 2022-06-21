---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 프로젝트 목록 조회

<p style="font-weight:bold">로그인한 사용자가 <font style="color: red;">참여 중인 프로젝트의 목록을 조회</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/selectProjectList/project-list_1.png"> <br>

! 제가 클라이언트라면 [프로젝트 목록 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/selectProjectList/project-list_2.png"> <br>

! [요구 사항 명세] 를 토대로 프로젝트 목록을 조회 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/byat/selectProjectList/project-list_5.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다.
! 로그인한 사용자가 포함된 프로젝트 목록, 프로젝트 구성원 목록을 조회하고 각 프로젝트의 시작, 종료 일자를 현재 날짜와 비교하여 진행 상태를 변경하여 보여지도록 구성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/selectProjectList/project-list_6.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 프로젝트 목록 조회 요청 시 로그인한 사용자가 참여 중인 프로젝트의 목록이 화면에 제대로 출력되는지 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 프로젝트 목록 조회에 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 프로젝트 목록 조회 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/selectProjectList/project-list_7.PNG"> <br>
-> 로그인 후 메인 페이지에서 상단 project 메뉴를 클릭합니다. <br>

<img src="../../../../img/byat/selectProjectList/project-list_8.PNG"> <br>
-> 클릭 시 프로젝트 목록 페이지로 이동합니다. 로그인한 사용자가 참여중인 프로젝트의 목록을 조회할 수 있습니다. 검색이 가능하며 한 페이지에 4개의 프로젝트를 확인할 수 있습니다.
프로젝트 이름, 프로젝트 구성원 목록(역할에 따라 색깔 구분, PM -> 빨강 | 부PM -> 연두 | 일반 멤버 -> 살색), 시작날짜, 종료날짜, 진행 상태, 담당자(PM)을 조회할 수 있습니다.

#### 코드

##### [Controller]
<img src="../../../../img/byat/selectProjectList/project-list_9.PNG"> <br>
-> session에 있는 로그인한 사용자의 정보를 service 메소드에 전달하여 프로젝트 목록을 반환받습니다. 그 후 구성원들을 화면에 표시할 때 이름만 표시하기 위해 반복문을 돌리며 substring으로 분리 후 저장하였습니다. 또한 담당자(PM)의 정보를 따로 List에 담아 view에 전달하도록 구현하였습니다. <br>

##### [ServiceImpl]
<img src="../../../../img/byat/selectProjectList/project-list_10.PNG">
<img src="../../../../img/byat/selectProjectList/project-list_11.PNG">
<img src="../../../../img/byat/selectProjectList/project-list_12.PNG"> <br>
-> controller에서 전달 받은 로그인한 사용자의 정보를 이용하여 Mapper 메소드를 이용하여 프로젝트 목록을 반환받습니다. 그 후 반복문으로 역할 순으로 정렬 되도록 구현하였습니다. 오늘 날짜와 시작, 종료 날짜를 비교하여 진행 상태가 변경되도록 조건을 사용하였으며 상태가 변경되었을 경우 변경 이력 또한 저장되도록 구현하였습니다. <br>

## [이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#list)



