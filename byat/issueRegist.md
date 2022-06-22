---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 이슈 제기

<p style="font-weight:bold">스프린트를 진행 중 <font style="color: red;">발생한 문제에 대해 이슈를 제기</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/issueRegist/issue-regist_1.PNG"> <br>

! 제가 클라이언트라면 [이슈 제기] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/issueRegist/issue-regist_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 이슈를 제기할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/byat/issueRegist/issue-regist_5.PNG">
<img src="../../../../img/byat/issueRegist/issue-regist_6.PNG"><br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다.
! 이슈를 먼저 생성한 뒤 초기 버전 히스토리, 상태 변경 이력, 구성원, 구성원 변경이력, 알림을 저장하도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/issueRegist/issue-regist_7.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 이슈 제기 요청이 제대로 수행되는지 직접 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 이슈 제기 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 이슈 제기 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/issueRegist/issue-regist_8.PNG"> <br>
-> 프로젝트 상세 페이지에서 진행 중인 스프린트를 클릭합니다.

<img src="../../../../img/byat/issueRegist/issue-regist_9.PNG"> <br>
-> 클릭 후 'Task 생성' 버튼을 클릭합니다. 클릭 시 태스크 생성 모달창이 조회됩니다.

<img src="../../../../img/byat/issueRegist/issue-regist_10.PNG"> <br>
-> 태스크 생성 모달창 우측 상단의 토글 버튼을 이용하여 이슈로 전환합니다. 클릭 시 이슈 생성 모달창으로 변환됩니다.

<img src="../../../../img/byat/issueRegist/issue-regist_11.PNG"> <br>
-> 이슈 생성 모달창에서 생성할 이슈 제목, 설명을 작성하고 중간 SelectBox를 이용하여 담당자를 지정합니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/byat/issueRegist/issue-regist_12.PNG"> <br>
-> 이슈 생성 시 담당자를 지정하도록 하였습니다. 비동기 ajax 를 사용하여 스프린트에 참여 중인 멤버만 선택할 수 있도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/byat/issueRegist/issue-regist_13.PNG"> <br>
-> view에서 전달 받은 새로운 이슈 정보와 로그인한 사용자의 정보를 변수에 담고 담당자 정보는 사번, 이름, 담당자 멤버 번호를 분리하여 저장하였습니다. 그 후 생성할 이슈 DTO에 정보를 setter로 저장하고 service 메소드에 전달하여 이슈를 생성하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/issueRegist/issue-regist_14.PNG">
<img src="../../../../img/byat/issueRegist/issue-regist_15.PNG"><br>
-> service와 이슈 생성 쿼리문입니다. Controller에서 전달 받은 이슈 정보와 초기 기본 세팅 정보를 사용하여 이슈를 생성합니다. 그 후 알림 정보를 setter로 저장하고 알림, 초기 히스토리, 변경 이력, 구성원, 구성원 변경 이력을 저장하도록 구현하였씁니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



