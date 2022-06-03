---
layout:     post
date:       2022-05-27
author:     InGeunPark
header-img: img/jaegojaego/thumbnail_jaego.png
catalog: true
---

[이전 페이지로](https://ingeunpark.github.io/2022/05/27/jaegojaego/#first)

# 가맹점 이슈 제기

## 가맹점이 발주 신청 물품을 실제로 받았을 때 물품에 대해 문제가 존재할 경우 이슈를 제기할 수 있는 기능입니다.

### 코드 설계 과정

#### [요구 사항 명세]
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_1.png"> <br>

제가 클라이언트라면 [가맹점 이슈 제기] 기능에 대해 어떠한 요구를 제시할 지 생각하고 작성한 [요구 사항 명세]입니다.

#### [단위 업무 정의서] 

<img src="../../../../img/jaegojaego/issueRegist/issue-regist_2.png"> <br>

[요구 사항 명세] 를 토대로 가맹점이 이슈를 제기 할 때 어떠한 업무가 필요할 것인지 생각하고 작성한 [단위 업무 정의서]입니다.

#### [데이터 베이스 논리 모델링]
- 초기 논리 모델링
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_3.png"> <br>

기본적으로 이슈 제기는 본사가 가맹점이 신청한 발주에 대해 출고를 하면 가맹점이 신청한 발주에 대해 물품을 받고, 그 물품이 문제가 생겼을 때 제기할 수 있습니다.
하지만 초기 논리 모델링에서는 단순히 발주, 출고와 연관짓지 않고 단순히 이슈로만 테이블을 구성하였기에 한 눈에 봐도 문제가 많아 보였습니다.

- 최종 논리 모델링
<img src="../../../../img/jaegojaego/issueRegist/issue-regist_4.png"> <br>

최종적으로는 발주에 대해 출고된 발주 물품에 대해 이슈를 제기할 수 있도록 하기 위해 출고와 연관지어 테이블을 구성하였습니다.




