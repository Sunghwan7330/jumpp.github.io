---
title: "[밑바닥부터 만드는 컴퓨팅] 프로젝트02 산술논리연산장치 제작"
excerpt: "해당 내용은 '밑바닥부터 만드는 컴퓨팅 시스템'  프로젝트02  부분을 정리하였습니다. "

categories:
  - 'make_computing_system'
tags:
  - 밑바닥부터 만드는 컴퓨팅

toc: true
toc_stic표ky: true

date: 2021-11-06
last_modified_at: 2021-11-16
---

# 개요 

해당 내용은 '밑바닥부터 만드는 컴퓨팅 시스템' 책의 프로젝트 02 의 내용을 정리하려 합니다. 
혹시나 따라하실 분은 실습준비 포스팅을 참고해서 프로그램을 다운받아주시면 됩니다. 

# 프로그램 실행 

다운받은 프로그램에서 `HardwareSimulator.bat` 를 실행해줍니다. 

![image](https://user-images.githubusercontent.com/35713051/142761388-9d8c0dc9-0cfb-4c4e-bdde-8b2fc20c87d3.png)

UI에서 보이는 화면 중 각 표시한 버튼은 아래와 같습니다. 

* ① : hdl 파일을 로드합니다. 
* ② : 스크립트를 실행합니다. 
* ③ : 스크립트를 로드합니다. 

# 반가산기 제작

반가산기는 두 비트 a와 b를 입력받아 더하여 `sum`과 `carry`를 계산하는 연산장치입니다. 

`sum`은 Xor 게이트로 구현이 가능합니다. 
Xor게이트처럼 `sum` 도 두 비트의 값이 같을때 0, 아닐때 1이 됩니다. 

`carry`는 And 게이트로 구현이 가능합니다. 
두 비트가 모두 1일때만 carry가 발생하기 때문입니다. 

따라서 아래와 같이 구현이 가능합니다.

```
CHIP HalfAdder {
    IN a, b;    // 1-bit inputs
    OUT sum,    // Right bit of a + b 
        carry;  // Left bit of a + b

    PARTS:
    // Put you code here:
    Xor(a=a, b=b, out=sum);
    And(a=a, b=b, out=carry);
}
```

# 전가산기 제작 

전가산기는 세개의 비트 `a`, `b`, `c` 를 입력받아 더하여 `sum` 과 `carry` 를 계산하는 연산장치입니다. 
세 개의 비트를 모두 더했을 때의 최대값은 3이므로 `sum` 과 `carry`를 통해 모든 값을 편할 수 있습니다. 

전가산기는 위에서 제작한 반가산기를 이용하여 쉽게 구현할 수 있습니다. 
전가산기를 이용하여 `a`와 `b`를 계산하고, 출력되는 `sum` 과 `c`를 반가산기로 더하여 나온 결과를 `sum`으로 출력합니다. 
이때 발생하는 `carry`들은 반가산기 구현할때와 같이 `Xor` 게이트를 이용하여 연산합니다. 
```
CHIP FullAdder {
    IN a, b, c;  // 1-bit inputs
    OUT sum,     // Right bit of a + b + c
        carry;   // Left bit of a + b + c

    PARTS:
    // Put you code here:
    HalfAdder(a=a, b=b, sum=sumab, carry=carryab);
    HalfAdder(a=sumab, b=c, sum=sum, carry=carryabc);
	Xor(a=carryabc, b=carryab, out=carry);
}
```

# Inc16 제작 

Inc16은 16비트를 입력받아 1을 더하는 연산장치입니다. 

이는 반가산기를 이용하여 간단히 구현이 가능합니다. 
최하위 비트와 1을 반가산기를 이용해 더하고 발생하는 carry를 다음 비트와 더하고, 발생하는 carry를 다음 비트와 더하고......
를 끝까지 반복해주시면 됩니다.  

```
CHIP Inc16 {
    IN in[16];
    OUT out[16];

    PARTS:
   // Put you code here:
    HalfAdder(a=in[0], b=true, sum=out[0], carry=c0);
    HalfAdder(a=in[1], b=c0, sum=out[1], carry=c1);
    HalfAdder(a=in[2], b=c1, sum=out[2], carry=c2);
    HalfAdder(a=in[3], b=c2, sum=out[3], carry=c3);
    HalfAdder(a=in[4], b=c3, sum=out[4], carry=c4);
    HalfAdder(a=in[5], b=c4, sum=out[5], carry=c5);
    HalfAdder(a=in[6], b=c5, sum=out[6], carry=c6);
    HalfAdder(a=in[7], b=c6, sum=out[7], carry=c7);
    HalfAdder(a=in[8], b=c7, sum=out[8], carry=c8);
    HalfAdder(a=in[9], b=c8, sum=out[9], carry=c9);
    HalfAdder(a=in[10], b=c9, sum=out[10], carry=c10);
    HalfAdder(a=in[11], b=c10, sum=out[11], carry=c11);
    HalfAdder(a=in[12], b=c11, sum=out[12], carry=c12);
    HalfAdder(a=in[13], b=c12, sum=out[13], carry=c13);
    HalfAdder(a=in[14], b=c13, sum=out[14], carry=c14);
    HalfAdder(a=in[15], b=c14, sum=out[15], carry=c15);   
}
```