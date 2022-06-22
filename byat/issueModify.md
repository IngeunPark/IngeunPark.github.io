---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 이슈 수정 및 담당자 변경

<p style="font-weight:bold">제기한 이슈의 <font style="color: red;">내용을 수정하거나 담당자를 변경</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/issueModify/issue-modify_1.PNG"> <br>

! 제가 클라이언트라면 [이슈 수정 및 담당자 변경] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/issueModify/issue-modify_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 이슈의 내용을 수정하거나 담당자를 변경할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

## [자세한 내용](https://www.notion.so/64f066b6ee4948f0926f0790b553dcad)

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/byat/issueRegist/issue-regist_3.png"> <br>

? 기본적으로 이슈는 담당자가 초기에 지정되며 여러 번 변경될 수 있는 작업이었습니다. <br>
? 하지만 초기 논리 모델링에서는 담당자와 담당자 변경 이력에 대한 테이블이 존재하지 않아 문제가 존재하였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/byat/issueRegist/issue-regist_4.PNG"> <br>

! 최종적으로는 이슈 구성원 테이블과 구성원 변경 이력 테이블을 관리하여 변경에 대한 관리를 할 수 있도록 마무리하였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/byat/issueModify/issue-modify_3.PNG">
<img src="../../../../img/byat/issueModify/issue-modify_4.PNG">
<img src="../../../../img/byat/issueModify/issue-modify_5.PNG">
<img src="../../../../img/byat/issueModify/issue-modify_6.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다. <br>
! 이슈 수정의 경우 두 가지 조건으로 나누어 작성하였습니다.
 1. 담당자에서 제외되었다가 다시 담당자로 지정된 경우
 2. 처음 담당자로 지정되는 경우
! 각 조건을 따져 이슈 내용을 수정하고 담당자의 참여 상태를 변경하고 알림, 변경 이력 등이 저장되도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/issueModify/issue-modify_7.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 이슈 수정 요청이 제대로 수행되는지 직접 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 이슈 수정 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 이슈 수정 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/issueModify/issue-modify_8.PNG"> <br>
-> 이슈 목록 페이지에서 수정할 이슈의 우측 상단 설정 버튼을 클릭 시 상세/조회 버튼과 삭제 버튼이 조회됩니다. 상세/조회 버튼을 클릭 시 모달창이 조회됩니다.

<img src="../../../../img/byat/issueModify/issue-modify_9.PNG"> <br>
-> 상세/조회 모달창에서 내용을 수정하거나, 담당자를 제외/추가 하여 이슈를 수정할 수 있습니다.

#### 코드

##### [Controller]
<img src="../../../../img/byat/issueModify/issue-modify_10.PNG"> <br>
-> view에서 전달 받은 수정할 이슈 정보와 로그인한 사용자의 정보를 변수에 담고 담당자 정보는 사번, 이름, 담당자 멤버 번호를 분리하여 저장하였습니다. 그 후 수정할 이슈 DTO에 정보를 setter로 저장하고 service 메소드에 전달하여 이슈를 수정하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/issueModify/issue-modify_11.PNG">
<img src="../../../../img/byat/issueModify/issue-modify_12.PNG">
<img src="../../../../img/byat/issueModify/issue-modify_13.PNG">
<img src="../../../../img/byat/issueModify/issue-modify_14.PNG"><br>
-> service와 기존 이슈 담당자 상태 변경 쿼리문입니다. Controller에서 전달 받은 이슈 정보를 사용하여 이슈 내용을 수정합니다. 그 후 이슈 담당자가 기존에 참여한 적이 있는지 조건을 따집니다. 
이미 참여한 적이 있는 담당자의 경우 참여 상태값을 Y로 변경하여 다시 담당자로 지정하고, 처음 담당자가 된 경우라면 새로 insert 하였습니다. 모두 알림 데이터가 저장되도록하였고, 변경 이력, 구성원, 구성원 변경 이력 또한 생성되도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



