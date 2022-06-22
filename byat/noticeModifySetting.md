---
layout:     post
date:       2022-05-16
author:     InGeunPark
header-img: img/byat/thumbnail_byat.png
catalog: true
---

# 알림 수신 설정 변경

<p style="font-weight:bold">로그인한 사용자가 <font style="color: red;">수신하고자 하는 알림 종류의 설정을 변경</font>할 수 있는 기능입니다. </p>

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_1.PNG"> <br>

! 제가 클라이언트라면 [알림 수신 설정 변경] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_2.PNG"> <br>

! [요구 사항 명세] 를 토대로 수신하고자 하는 알림 종류의 설정을 변경할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

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

<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_3.PNG"> <br>

! 하나의 컨트롤러에선 하나의 서비스 메소드만 호출하도록 작성하였으며 Mapper 메소드, xml을 통하여 쿼리를 실행하고 값을 반환받도록 진행하였습니다. <br>
! 알림 수신 설정 변경 요청을 하고 성공할 시 성공 값을 반환 받도록 작성하였습니다.

#### [통합 테스트]

<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_4.PNG"> <br>

! url로 요청 시 기능이 문제 없이 동작하는지 확인하기 위해 통합 테스트를 진행하였습니다. <br>
! 알림 수신 설정 변경 요청이 제대로 수행되는지 직접 확인하여 테스트 하였습니다.

#### Result
! [요구 사항 명세]로 클리이언트가 알림 수신 설정 변경 대해 제시할 만한 요구사항을 파악해 보았고, 이를 바탕으로 알림 수신 설정 변경 단위의 [단위 업무 정의서]를 작성하였습니다.  <br>
! 설계 단계에서 지속적인 수정을 통해 설계한 최종 [논리 모델링]으로  [시퀀스 다이어그램]을 작성한 뒤 이를 바탕으로 구현을 하였습니다. <br>
! 최종적으로 [통합 테스트]를 진행하여 프로젝트에 문제가 없는지 확인하였습니다.

### 화면 및 코드 설명

#### 화면
<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_5.PNG"> <br>
-> 알린 간단 목록창 좌측 하단 톱니바퀴 모양 버튼을 클릭하여 설정 페이지로 이동할 수 있습니다.

<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_6.PNG"> <br>
-> 알림 설정 페이지에서 toggle 버튼을 이용하여 알림을 수신 여부를 변경할 수 있습니다. ON의 경우 알림을 수신하고 OFF의 경우 알림을 수신하지 않습니다.

#### 코드

##### [JavaScript]
<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_7.PNG"> <br>
-> 알림 toggle 버튼을 클릭할 시 비동기 ajax로 사용자 멤버 번호 알림 설정 변경 상태를 담아 요청합니다. 

##### [Controller]
<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_8.PNG"> <br>
-> view에서 전달 받은 정보를 사용자 멤버 번호를 NoticeSettingDTO에 담고 알림 수신 설정 정보를 각 변수에 저장합니다. 그 후 조건문으로 어떠한 알림에 대한 설정 변경인지와 어떤 설정으로 변경인지 확인하여 변수에 담고 service 메소드에 전달하여 알림 설정을 변경하도록 구현하였습니다.

##### [ServiceImpl]
<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_9.PNG">
<img src="../../../../img/byat/noticeModifySetting/notice-modify-setting_10.PNG"><br>
-> service에서 알림 설정 변경 쿼리문을 실행하고 제대로 완료되었는지 Exception으로 확인하였습니다. 알림 설정 변경 쿼리문에선 if문으로 조건을 따져 전달 받은 알림 설정을 변경하도록 구현하였습니다.

## [이전 페이지로](https://ingeunpark.github.io/2022/05/16/byat/#list)



