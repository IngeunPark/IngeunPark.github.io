---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 이슈 처리 상태 변경

<p style="font-weight:bold">이슈의 처리 상태를 <font style="color: red;">해결 전, 해결 중, 완료로 변경</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/issueModifyStatus/issue-modify-status_1.PNG"> <br>

! 제가 클라이언트라면 [이슈 처리 상태 변경] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/issueModifyStatus/issue-modify-status_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 이슈 처리 상태 변경할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

## [자세한 내용](https://www.notion.so/64f066b6ee4948f0926f0790b553dcad)

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/byat/issueRegist/issue-regist_3.PNG"> <br>

? 기본적으로 이슈는 담당자가 초기에 지정되며 여러 번 변경될 수 있는 작업이었습니다. <br>
? 하지만 초기 논리 모델링에서는 담당자와 담당자 변경 이력에 대한 테이블이 존재하지 않아 문제가 존재하였습니다.

- 최종 논리 모델링 <br>
<img src="../../../../img/byat/issueRegist/issue-regist_4.PNG"> <br>

! 최종적으로는 이슈 구성원 테이블과 구성원 변경 이력 테이블을 관리하여 변경에 대한 관리를 할 수 있도록 마무리하였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/byat/issueModifyStatus/issue-modify-status_3.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다. <br>
! 이슈의 처리 상태를 변경할 때 변경 이력을 저장하고 이슈의 변경자 정보를 변경하며 알림이 발생하도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/issueModifyStatus/issue-modify-status_4.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 이슈 처리 상태 변경 요청이 제대로 수행되는지 직접 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 이슈 처리 상태 변경 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 이슈 처리 상태 변경 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/issueModifyStatus/issue-modify-status_5.PNG"> <br>
-> 이슈 목록 페이지에서 처리 상태를 변경할 이슈를 클릭 후 드래그 합니다.

<img src="../../../../img/byat/issueModifyStatus/issue-modify_status_6.PNG"> <br>
-> 변경할 처리 상태 공간으로 드롭하여 처리 상태를 변경할 수 있습니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/byat/issueModifyStatus/issue-modify_status_7.PNG"> <br>
-> 변경할 이슈에 대해 addEventListner를 사용하여 드래그 시작한 이슈를 화면에 표시되도록 하였고 드래그가 끝났을 때 드롭한 공간에 이슈가 나타나도록 구현하였습니다.

<img src="../../../../img/byat/issueModifyStatus/issue-modify_status_8.PNG"> <br>
-> 드롭 이벤트가 발생할 경우 비동기 ajax를 통하여 처리 상태 변경을 요청하도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/byat/issueModifyStatus/issue-modify_status_9.PNG"> <br>
-> view에서 전달 받은 변경할 이슈의 코드와 변경할 처리 상태, 로그인한 사용자의 정보를 IssueDTO에 setter를 통해 저장하고 service 메소드에 전달하여 이슈 처리 상태를 변경하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/issueModifyStatus/issue-modify_status_10.PNG">
<img src="../../../../img/byat/issueModifyStatus/issue-modify_status_11.PNG"><br>
-> service와 이슈 처리 상태 변경 쿼리문입니다. Controller에서 전달 받은 이슈 정보를 사용하여 이슈 처리 상태를 변경합니다. 그 후 이슈 처리 상태 변경자의 정보를 저장하고 이슈 담당자들의 목록을 조회하여 처리 상태 변경 알림을 저장하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



