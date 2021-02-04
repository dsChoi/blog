---
title: "[deview] 네이버 모델서빙"
date: 2020-11-30 10:57:53
categories: blog
showDate: true
tags: [deview, ML]
---

#### 목차

##### 1. 모델서빙 공통화 동기

- 모델 수의 증가
- 지속적인 모델 학습과 배포
  - 지속적으로 학습을 하면 배포가 필요한데 모델 배포한후 모델에 대한 a/b 테스트
  - 로그로 인한 학습이 필요
  - 서빙에서 가능한 부분은 작업
- 커스텀한 사내 시스템 연동
  - 사내플랫폼 시스템이 다양하기에 
  - 불필요한 코드가 중복되지 않도록 정리
- 협업 가능한 환경 구축 (모델러 / 엔지니어)
  - 모델이 늘어날수록 엔지니어가 작업할 내용이 많아짐

![image-20201125135008717](/Users/stones/git/dsChoi.github.io/source/image-20201125135008717.png)

* gateway - 인증, 로깅, 웨이트

* model sdk 
  * 모델러와 협업할수 있는 인터페이스 제공 /GPC 등에 업로드할 수 있는
  * 모델 디스커버리, 로깅, 헬스체크, 게이트웨이와 연동되는 로직을 넣음

* deploy - 모델의 디플로이를 맡음 
  * 이종 시스템이 많아 디플로이먼트가 인터페이스를 제공하여 단일한 인터페이스를 제공함

##### 2. MDS: 공통 모델 패키징 - 김병훈

### 서빙 환경 및 요구사항

- 다양한 ML 프레임워크 - 텐서플로우
  - Base 이미지 프레임워크 별로 
- REST API 로 서비스 제공
  - Decorator /Flask
- GPU - 고성능
  - 오퍼레이터 / 배치 처리. 기능 
- Docker - 종속성 배포 문제 해결을 위해 사용
  - 배포는 Docker



### 일반적인 인퍼런스 요청 처리 순서

![image-20201125135517498](/Users/stones/Library/Application Support/typora-user-images/image-20201125135517498.png)



- 요청 -> 전처리 -> 인퍼런스 -> 후처리 -> 응답

### 서빙 핸들러 구현

![image-20201125135558960](/Users/stones/git/dsChoi.github.io/source/image-20201125135558960.png)

### 패키지 생성 및 서빙

![image-20201125135707546](/Users/stones/Library/Application Support/typora-user-images/image-20201125135707546.png)



### 배치 인퍼런스

![image-20201125135736125](/Users/stones/git/dsChoi.github.io/source/image-20201125135736125.png)

### 멀티 모델 파이프라인

![image-20201125135800752](/Users/stones/git/dsChoi.github.io/source/image-20201125135800752.png)



##### 3. DPLO: 공통 모델 배포

### 모델 배포 서버 요구 및 구현

![image-20201125135916285](/Users/stones/git/dsChoi.github.io/source/image-20201125135916285.png)

### 배포 과정

![image-20201125135938081](/Users/stones/git/dsChoi.github.io/source/image-20201125135938081.png)





### 배포 오퍼레이터

### 배포 요청 Scheme

![image-20201125140140264](/Users/stones/git/dsChoi.github.io/source/image-20201125140140264.png)

### 선언 파일을 통한 배포

![image-20201125140234170](/Users/stones/git/dsChoi.github.io/source/image-20201125140234170.png)



### 동적 Endpoint Discovery

![image-20201125140331854](/Users/stones/git/dsChoi.github.io/source/image-20201125140331854.png)

![image-20201125140453940](/Users/stones/git/dsChoi.github.io/source/image-20201125140453940.png)

##### 4. AIGW: 공통 모델 라우팅

- 라우팅 환경 및 요구사항

- 라우팅 Scheme 및 로드 밸런싱 방식

![image-20201125140526109](/Users/stones/git/dsChoi.github.io/source/image-20201125140526109.png)

- 동적 URL 매칭 라우터

  ![image-20201125140659278](/Users/stones/git/dsChoi.github.io/source/image-20201125140659278.png)

- 요청 미러링 기능

![image-20201125140714885](/Users/stones/git/dsChoi.github.io/source/image-20201125140714885.png)

- 가중치 기반 라우팅

![image-20201125140808348](/Users/stones/git/dsChoi.github.io/source/image-20201125140808348.png)

- 모델 로그 유통 및 관리

![image-20201125140920393](/Users/stones/git/dsChoi.github.io/source/image-20201125140920393.png)
- 로그 시각화

![image-20201125141013780](/Users/stones/git/dsChoi.github.io/source/image-20201125141013780.png)



- 모델 서빙 모니터링

![image-20201125141000187](/Users/stones/git/dsChoi.github.io/source/image-20201125141000187.png)

##### 5. Recap & Conclusion - 서동필

- 더 편리하고 효과적인 모델 서빙/운영 경험 제공

![image-20201125141105831](/Users/stones/git/dsChoi.github.io/source/image-20201125141105831.png)