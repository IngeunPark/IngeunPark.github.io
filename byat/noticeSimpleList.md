---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 알림 간단 목록 조회

<p style="font-weight:bold">로그인한 사용자가 <font style="color: red;">수신한 알림 목록을 nav바에서 간단히 조회</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_1.PNG"> <br>

! 제가 클라이언트라면 [알림 간단 목록 조회] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 현재 페이지에서 알림 목록을 조회할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

## [자세한 내용](https://www.notion.so/64f066b6ee4948f0926f0790b553dcad)

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링 <br>
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_3.png"> <br>

? 기본적으로 알림은 사용자가 참여 중인 프로젝트 관련의 작업이 발생할 경우 데이터가 저장되는 방식입니다. <br>
? 하지만 초기 논리 모델링에서는 하나의 테이블에 모든 컬럼이 모여있을 뿐만 아니라 멤버 개인별 알림 설정도 하지 못하는 상황이었습니다. 

- 최종 논리 모델링 <br>
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_4.PNG"> <br>

! 최종적으로는 멤버별 알림 설정 테이블을 만들고 구분 테이블을 분리하여 더욱 효율적인 구성으로 변경하였습니다.

#### [시퀀스 다이어그램]

<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_5.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다. <br>
! 알림을 조회할 때 조회하지 않기로 한 알림을 구분해야 하기 때문에 알림 설정 또한 조회하여 비교할 수 있도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_6.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 알림 조회 요청이 제대로 수행되는지 직접 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 알림 간단 조회 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 알림 간단 조회 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_7.PNG"> <br>
-> 페이지 nav 바에서 종 모양 아이콘을 클릭하여 알림 간단 목록 조회를 할 수 있습니다. 참여 중인 프로젝트 관련 작업이 수행될 경우 알림이 생성되며 종 모양 우측 상단에 숫자가 추가됩니다. 숫자는 조회하지 않은 알림의 숫자만 표시합니다.

<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_8.PNG"> <br>
-> 알림 목록 창이 조회되며 발생한 알림의 내용을 조회할 수 있습니다. 이미 조회한 알림은 회색 바탕으로 표시됩니다. 알림 중 하나를 클릭 시 알림이 발생한 페이지로 이동할 수 있습니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_9.PNG">
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_10.PNG">
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_11.PNG"> <br>
-> 실시간 알림을 위한 JavaScript 코드입니다. WebSocket을 사용하여 실시간 통신이 가능하도록 구현하였습니다. 페이지가 로드될 때 마다 ajax를 사용하여 알림을 조회하였고 종 모양 아이콘을
클릭할 시 socket 통신을 통해 실시간으로 알림을 조회하도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_12.PNG"> <br>
-> view에서 전달 받은 사용자 번호를 service 메소드에 전달하여 사용자에 해당하는 알림 목록을 조회하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_13.PNG">
<img src="../../../../img/byat/noticeSimpleList/notice-simple-list_14.PNG"><br>
-> service와 알림 목록, 알림 설정 조회 쿼리문입니다. Controller에서 전달 받은 멤버 번호를 통하여 멤버에 해당하는 알림을 조회합니다. 그 후 멤버 알림 설정을 조회 후 switch 문을 
사용하여 조회할 알림만 확인하여 새로운 List에 저장하고 반환하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



