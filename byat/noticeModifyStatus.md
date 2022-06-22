---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 알림 읽음 상태 변경

<p style="font-weight:bold">로그인한 사용자가 <font style="color: red;">수신한 알림의 읽음 상태를 변경</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_1.PNG"> <br>

! 제가 클라이언트라면 [알림 읽음 상태 변경] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 알림의 읽음 상태 변경할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_3.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다. <br>
! 알림의 읽음 상태를 변경 요청하고 반환 받도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_4.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 알림 삭제 요청이 제대로 수행되는지 직접 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 알림 읽음 상태 변경 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 알림 읽음 상태 변경 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/noticeRemove/notice-remove_5.PNG"> <br>
-> 알림 전체 목록 페이지에서 읽음 상태 변경할 알림에 마우스를 올리면 설정 버튼이 조회됩니다.

<img src="../../../../img/byat/noticeRemove/notice-remove_6.PNG"> <br>
-> 설정 버튼 클릭 시 알림 읽음 상태 버튼이 조회됩니다. 읽은 알림의 경우 읽지 않음 상태로 변경 버튼이 조회되며 읽지 않은 알림의 경우 읽음 상태로 변경할 수 있는 버튼이 조회됩니다.

<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_5.PNG"> <br>
-> 버튼을 클릭 시 상태 변경이 완료됩니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_6.PNG"> <br>
-> 상태 변경 버튼을 클릭 시 비동기 ajax로 사용자 멤버 번호, 알림 코드, 변경할 알림 상태를 전달하여 요청하고 성공 시 설정 버튼이 반대로 변경되도록 구현하였습니다.

##### [Controller]
<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_7.PNG"> <br>
-> view에서 전달 받은 정보를 NoticeDTO에 setter를 사용하여 저장한 뒤 service 메소드에 전달하여 알림 읽음 상태 변경 작업을 진행하도록 구현하였습니다. 

##### [ServiceImpl]
<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_8.PNG">
<img src="../../../../img/byat/noticeModifyStatus/notice-modify-status_9.PNG"><br>
-> service에서 읽음 상태 변경 쿼리문을 수행하고 읽음 상태 변경 쿼리문이 제대로 수행되었는지 Exception으로 확인하였습니다. 쿼리문으로 알림의 상태를 조건을 따져 기존의 상태와 반대로 설정되도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



