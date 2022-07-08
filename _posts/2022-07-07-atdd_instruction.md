---
title: "우아한 ATDD"
excerpt: "유튜브 '우아한 Tech' 채널의 '우아한 ATDD' 정리본입니다. "

categories:
  - 'infomation'
tags:
  - atdd
  - tdd
  - agile

date: 2022-07-07
last_modified_at: 2022-07-07
---

# 개요 

이번 포스팅은 `우아한 Tech` 채널에서 강의한 우아한 ATDD 에 대한 내용을 정리하려 합니다.

* https://youtu.be/ITVpmjM4mUE

이번 포스팅은 개조식으로 정리하겠습니다. 

대부분은 강의자료를 가져왔습니다. 

# 인수테스트 첫 만남 

* 해당 절에서는 인수테스트를 접했을 떄의 느낌을 정리함

##  새로운 팀 새로운 문화 

### 페어 프로그래밍 

* 즉각적인 피드백 받을 수 있었음 

### 테스트 

* 테스트 코드로 부터 받는 피드백
* 심리적인 안정감을 제공함 

### 인수 테스트 

* 사용자 스토리(시나리오) 기반으로 기능 테스트 

### 인수 테스트의 도움 

* 배표 없이 받는 빠른 피드백
* 새로운 팀의 도메인과 서비스 흐름 파악에 큰 도움이 됨 
* 도메인을 이해하는데 예상보다 짧은 시간이 소요
* 인수 테스트로 스펙 표현 가능 

### 빠른 피드백

* 새로운 문화들의 공통점은 빠른 피드백을 받는 방법
* 든든한 지원군과 함께 코딩을 하는 느낌을 받음 
* 자신감을 주는 자동화된 테스트 

###  피드백을 받는 방법 

* 페어 프로그래밍 
* 테스트 / 인수 테스트 
* 코드 리뷰 
* 배포 
* 출시 

* 이 중 즉각적인 피드백을 줄 수 있는 테스트가 효과적이라고 생각함 

### 테스트 주도 개발 

* 테스트를 설계 활동으로 바꾸는 효과도 있음 
* 이로 인해 설계 품질에 관한 피드백도 빠르게 받게 도와줌
* 기존에는 검증의 목적으로 테스트를 했다면, TDD는 좋은 설계를 할 수 있도록 도움을 줌

### 인수 테스트 주도 개발 

* 개발 전 인수 테스트 작성 
* 인수 테스트 통과를 위한 개발 

### 인수 테스트를 기반으로 개발을 할 경우 

* 기존 인수 테스트 장점 
  * 빠른 피드백을 받을 수 있음 
  * 회귀 오류를 잡아줄 꾸준한 테스트를 만들 수 있음 
  * 기존 기능을 망가뜨리지 않고 새 기능을 추가할 수 있음 
* 인수 테스트를 작성하면서 구현할 대상에 대한 이해도 증가 
* 작업의 시작과 끝이 명확해져서 심리적인 안정감에 도움을 줌 

# ATDD란 

## 테스트 vs TDD

[그림]

* 테스트는 기존의 기능을 검증하는 용도 
*  TDD는 구현의 요구사항을 명세하기 위해 테스트를 사용함 

## TDD vs BDD

[그림]

* BDD 는 행위를 기반으로 테스트를 작성해 나가는 방식

## TDD vs BDD vs ATDD

[그림]

* ATDD 는 요구사항의 명세를 인수테스트로 명세함

## 다 같이 인수 조건을 정의하는 이슈 

* 각자의 생각만으로 개발하면 기획자, 개발자, QA 전부 다른 기능을 생각할 수 있음 
* 개발 전 다 같이 인수 조건을 정의한 뒤 개발을 진행하는 것이 중요함 

## ATDD 프로세스 

[그림]

* TDD에 비해 복잡한 프로세스를 가짐
* 본 강의에서는 개발 관련된 내용만 다룸 