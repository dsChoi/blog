---
title: "몽고디비란?"
date: 2021-02-08T17:21:08+09:00
showDate: true
draft: false
tags: ["몽고디비", "mongdoDB", "mongo"]  
---

# 몽고디비란?

**MongoDB의 핵심 기능**

1. 도큐먼트 데이터 모델
2. 애드훅 쿼리
3. 인덱스
4. 복제
5. 속도와 내구성
6. 확장

**데이터 베이스 패밀리**


|                         | 예                                                         | 데이터 모델                                                  | 확장 모델                                                    | 용례                                                     |
| ----------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------------------------- |
| 간단한 키-값 저장시스템 | 맴캐시디                                                   | 키-값, 여기서 값은 이진 blob                                 | 여러가지가 있다. 맴캐시디는 이용 가능한 램으로 노드에 걸쳐 확장함으로써 하나의 단일한 데이터 스토어로 변한다. | 캐싱, 웹 ops                                             |
| 정교한 키-값 저장시스템 | 카산드라, 볼드모트 프로젝트(Project Voldemort), 리악(Riak) | 여러가지가 있다. 카산드라는 칼럼으로 부르는 키-값 구조를 사용하고, 볼트모트는 이진 blob을 사용한다. | 높은 확장성과 용이한 장애조치를 위한 Eventually-sonsistent, 다중 노드 분산 | 고효율 verticals(액티비티 피드, 메시지 큐), 캐싱, 웹 ops |
| 관계 데이터베이스       | 오라클, MySQL, PostgreSQL                                  | 테이블                                                       | 수직적 확장, 제한적인 클러스터링과 수동적인 파티션           | 트랜잭션이 필요한 시스템 또는 SQL, 정규화된 데이터모델   |



mongoDB 의 문제점

* 사용하는 데이터 구조가 데이터 크기 관점에서는 효율적이지 않다.
  * 'username'이라는 필드 이름을 가진 모든 도큐먼트가 필드 이름을 저장하기 위해서는 8바이트를 사용함
* MongoDB 쿼리가 SQL 만큼 친숙하거나 쉽지 않다.





> **요약**
>
> MongoDB 의 특징
>
> * 동적 쿼리
> * 세컨더리 인덱스
> * 원자적 업데이트와 복잡한 집계
> * 자동 장애조치와 복제
> * 수평적 확장을 위한 샤딩