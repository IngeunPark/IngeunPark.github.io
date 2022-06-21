---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 프로젝트 구성원 관리

<p style="font-weight:bold">권한이 PM인 멤버가 <font style="color: red;">프로젝트의 구성원을 관리(추가, 제외, 역할 수정)</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_1.PNG"> <br>

! 제가 클라이언트라면 [프로젝트 구성원 관리] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 프로젝트의 구성원을 관리할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_3.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_4.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_5.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_6.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다. <br>
! 프로젝트 구성원 추가, 제외, 역할변경을 구분하여 시퀀스 다이어그램을 작성하였습니다. 추가의 경우 이미 한 번 추가된 경우를 따져야 하기 때문에 두 가지 경우로 나눠 작성하였습니다. <br>
! 제외 시에는 제외하려는 멤버가 진행 중인 스프린트가 존재하는지 확인 후 제외하도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_7.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 프로젝트 구성원 관련 요청이 제대로 수행되는지 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 프로젝트 구성원 관리 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 프로젝트 구성원 관리 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_8.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_9.PNG"><br>
-> 프로젝트 목록에서 팀원 추가 버튼을 클릭 시 모달창이 조회됩니다. 이름 또는 사번을 통해 검색할 수 있으며 선택시 구성원 목록에 추가됩니다. 역할 또한 변경할 수 있습니다. <br>

<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_8.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_10.PNG"><br>
-> 프로젝트 목록에서 참여자 목록의 더보기(...) 버튼을 클릭 시 구성원 목록 모달창이 조회됩니다. 프로젝트 역할이 PM인 사용자는 구성원의 역할을 변경하거나 구성원에서 제외할 수 있습니다. 이 때 진행 중인 스프린트가 존재하는 멤버나 PM은 제외할 수 없습니다.

#### 코드

##### [Controller]
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_11.PNG"> <br>
-> 구성원 추가 Controller 코드입니다. 멤버의 대한 정보를 ModelAttribute를 사용하고 view 에서 전달 받은 정보들을 setter를 통해 저장합니다. service에 전달하여 구성원 추가 작업을 진행하도록 구현하였습니다.

<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_12.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_13.PNG"><br>
-> 구성원 제외, 역할 변경 코드입니다. 제외의 경우 비동기 방식으로 진행되도록 하였으며 제외의 경우 프로젝트 프로젝트 번호와 멤버 번호, 역할 변경은 변경할 역할을 service에 전달하여 작업을 수행하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_14.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_15.PNG"> <br>
-> 구성원 추가 service 코드입니다. 먼저 이미 추가된 적이 있는 멤버인지 확인 후 추가된 적이 있으면 데이터의 상태만 변경하고 없을 경우 새로 추가하도록 구현하였습니다. 또한 상태 변경 이력, 일정 테이블에 데이터를 추가로 저장하도록 구현하였습니다.

<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_16.PNG">
<img src="../../../../img/byat/projectMemeberManagement/project-memberManagement_17.PNG"> <br>
-> 구성원 제외, 역할 변경 service 코드입니다. 제외는 controller에서 전달 받은 멤버 정보를 사용하여 참여 상태값을 변경하도록 구현하였습니다.
구성원 역할 변경의 경우 이전 멤버들의 정보를 조회하여 역할이 변경된 멤버들로 새로 List에 담고 역할을 변경하도록 하였고 변경 이력 또한 저장하도록 구현하였습니다. 또한 PM이 변경될 경우 프로젝트 PM 데이터 또한 변경되도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



