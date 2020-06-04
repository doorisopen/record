---
title: 유클리드 호제법
category: Algorithm
comments: true
order: 7
---

## 목차
* 유클리드 호제법(최대공약수, GCD)
* 최소 공배수(LCM)


# 유클리드 호제법(알고리즘)이란?
* 주어진 두 수 사이에 존재하는 최대공약수(GCD)를 구하는 알고리즘 입니다.
* GCD, Greater Commona divisor

## 원리
* 임의의 두 자연수 a=45, b=30이 주어졌을때
* a를 b로 나눈 나머지를 m 이라고 하면 (a%b = m) 이다.
* 이때 m이 0일때, b가 최대 공약수(GCD)이다.
* 만약 m이 0이 아니라면, a에 b값을 다시 넣고(a=b) b에 m을(b=m) 대입 한 후 b가 0이 될때까지 a%b 연산을 반복한다.

```
//==반복문을 이용한 GCD==//
int gcd(int a, int b) {
    int tmp;
    if(a<b) { // a에 큰값을 위치 시킨다.
        tmp = a;
        a = b;
        b = tmp;
    }
    int m;
    while(b != 0) { // 유클리드(GCD) 알고리즘
        m = a % b;
        a = b;
        b = m;
    }
    return a;
}

//==재귀을 이용한 GCD==//
int gcd(int a, int b) {
    if(b == 0) {
        return a;
    } else {
        return gcd(b, a%b);
    }
}
```

# 최소 공배수(LCM)
* __공배수__ 란 2개 이상의 자연수의 공통된 배수이다.
* 공배수 중에서 가장 작은 공배수를 __최소 공배수(LMC)__ 라고 한다.
* 최소 공배수는 최대 공약수를 활용하여 쉽게 구할 수 있다.
* a, b 두 수가 주어졌을때 a*b을 최대 공약수(gcd(a, b))로 나눠주면 된다. 

```
//==최대 공약수 GCD==//
int gcd(int a, int b) {
    int tmp;
    if(a<b) { // a에 큰값을 위치 시킨다.
        tmp = a;
        a = b;
        b = tmp;
    }
    int m;
    while(b != 0) { // 유클리드(GCD) 알고리즘
        m = a % b;
        a = b;
        b = m;
    }
    return a;
}

//==최소 공배수 LCM==//
int lcm(int a, int b) {
    return (a * b) / gcd(a, b);
}
```