---
title: "[밑바닥부터 만드는 컴퓨팅] 불 연산"
excerpt: "해당 내용은 '밑바닥부터 만드는 컴퓨팅 시스템' 책의 2장 내용을 정리하였습니다. "

categories:
  - 'make_computing_system'
tags:
  - 밑바닥부터 만드는 컴퓨팅
  - 불 연산

toc: true
toc_sticky: true

date: 2021-11-10
last_modified_at: 2021-11-10
---

# 개요 

해당 내용은 '밑바닥부터 만드는 컴퓨팅 시스템' 책의 2장 내용을 정리하였습니다.

이번 장에서는 숫자를 표현하고 계산하는 논리게이트를 설계합니다. 
1장에서는 기본적인 논리 게이트를 만들었다면, 이번장에서는 산술 논리 연산장치를 만드는 것을 목표로 합니다. 
ALU는 컴퓨터의 모든 산술 및 논리 연산이 이루어지는 집이며, ALU를 구현하여 컴퓨터가 어떻게 작동하는지 이해할 수 있습니다. 



# 1. 배경

## 1.1 2진수

2진수는 기수(base)를 2로 합니다. 
`1100` 이라는 이진수가 있다면 다음과 같이 10진수로 변환할 수 있습니다. 

```
(1*8) + (1*4)) + (0*2) + (0*1) = 8 + 4 = 12
```

## 1.2 2진 덧셈

2진수의 덧셈은 어릴 때 배운 10진수의 덧셈 방법과 같습니다. 
다만 자릿수가 바뀌는 값이 10이 아닌 2라는 것이 차이입니다. 
가장 오른쪽부터 더하고, 값이 2을 넘을 때 자릿수를 넘겨 계산하면 됩니다. 
이러한 방식으로 가장 높은 자리까지 더해주면 됩니다. 

만약 마지막 비트를 더하고 나서도 자리올림수가 1이라면 overflow가 발생합니다. 

```
* overflow 발생 없는 경우 


  1 0 0 1
+ 0 1 0 1
-----------
0 1 1 1 0


* overflow 가 발생하는 경우 

  1 0 1 1 
+ 0 1 1 1
-----------
1 0 0 1 0
```

## 1.3 부호가 있는 2진수

n비트의 수로 표현 가능한 비트는 총 2^n개입니다. 
예를들어 2비트라면 표현가능한 값은 `00`, `01`, `10`, `11``으로 총 2^2개입니다.
만약 부호를 표시해야 한다면 표현할 수 있는 두개의 집합으로 나누는 것이 자연스러울 것입니다. 
또한 하드웨어 구현이 가능한 덜 복잡하도록 구성하는것이 좋을 것입니다. 

이러한 문제를 해결하기 위해 2진 표현법에서 부호있는 숫자를 표현하는 여러가지 방법이 개발되었습니다. 
그 중 오늘날 거의 대부분의 컴퓨터들은 2의 보수법 이라는 방법을 사용합니다. (컴퓨터구조 시간에 들어봤죠?)

2의 보수법의 음수표현은 해당 비트의 가장 큰 값에서 표현할 값을 빼서 계산합니다. 
만약 4비트 2진수에서 -2를 나타내는 2의 보수는 아래와 같습니다. 

```
1111 - 0010 = 1101
```

이러한 2의 보수법은 아래와 같은 특성을 가집니다. 

| 양수 | 음수 | 
| --- | --- |
| 0 :0000| |
| 1 : 0001 | -1 : 1111 |
| 2 : 0010 | -2 : 1110 |
| 3 : 0011 | -3 : 1101 |
| 4 : 0100 | -4 : 1100 |
| 5 : 0101 | -5 : 1011 |
| 6 : 0110 | -6 : 1010 |
| 7 : 0111 | -7 : 1001 |
|  | -8 : 1000 |

* 이 방식으로 총 2^n개의 수를 표현할 수 있음
* 모든 양의 코드는 0으로 시작함
* 모든 음의 코드는 1로 시작함

저는 학교다닐 때 이 2의보수법을 왜 사용하는지 몰랐습니다. 
하지만 지금에서는 이 방법이 하드웨어적으로 계산하기 가장 편한 방법이라고 생각이 됩니다. 
2의 보수법은 가산기, 즉 덧셈만으로 연산을 할 수 있습니다. 

`1 - 1`은 `1 + (-1)` 과 같습니다. 
이를 2진 계산법으로 아래와 같이 표현할 수 있습니다. 
```
0001 + 1111 = 0000
```
캐리를 무시하고 덧셈을 하면 0이 되죠. 

한번 더 해보도록 하겠습니다. 
`5 - 3`은 `2`입니다. 
이진 계산법으로 아래와 같이 표현할 수 있습니다. 

```
0101 + 1101 = 0010
```

이처럼 2의 보수법은 가산기만을 이용하여 뺄셈을 계산할 수 있게됩니다. 

# 2. 명세

## 2.1 가산기 

다음 세 개의 가산기 칩들은 멀티비트 가산기 칩으로 이어집니다. 

* 반가산기 (half-adder) : 두 비트를 더함
* 전가산기 (full-adder) : 세 비트를 더함
* 가산기 (adder) : 두 개의 n비트 숫자를 더함

### 2.1.1 반가산기 

반가신기는 두 비트를 더하는 기능을 합니다. 
덧셈안 값의 최하위 비트를 sum, 최상위 비트를 carry라고 합니다. 
`1+1`이라면 sum은 0, carry는 1이 되겠습니다. 

### 2.1.2 전가산기 

반가산기와 마찬가지로 전가산기 칩의 출력은 덧셈 값의 최하위 비트와 자리올림 비트의 2개입니다. 

### 2.1.3 가산기 

