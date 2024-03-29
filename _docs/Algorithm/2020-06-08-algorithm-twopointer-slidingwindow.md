---
title: 투 포인터(Two Pointers)와 슬라이딩 윈도우
category: Algorithm
date:   2020-06-08 00:30:59
comments: true
order: 37
---

## 투 포인터(Two Pointers) 란?
* __투 포인터__ 는 2개의 포인터를 조작해가며 원하는 것을 얻는 형태이다.
* 대표적인 문제는 [이것](https://www.acmicpc.net/problem/2003) 이 있다.

## 슬라이딩 윈도우(Sliding Window)란?
* __슬라이딩 윈도우는__ 투 포인터와 비슷한 유형이다.
* 투 포인터와 차이점은 __어느 순간에도 그 구간의 넓이가 동일하다는__ 차이점이 있다.


## 투 포인터 예시
https://www.acmicpc.net/problem/2003 문제를 예시로 투 포인터 방식을 설명해 보겠다.


__N개의 수로 된 수열__ A[1], A[2], …, A[N] 이 있다. 이 수열의 __i번째 수부터 j번째 수까지의 합__ A[i]+A[i+1]+…+A[j-1]+A[j]가 __M이 되는 경우의 수__ 를 구하는 프로그램을 작성하시오.

1. N개의 수열
2. i번째 수 ~ j번째 수까지의 합이 M
3. M의 경우의 수


## 접근 방법

N=10 M=5 일떄

A[10] = {1, 2, 3, 4, 2, 5, 3, 1, 1, 2} 이와 같은 수열이 있다.

1. __2개의 포인터 ☆와 ★을__ 준비한다.
2. 만약 ☆부터 ★까지의 부분합이 M 이상이거나, ★ = N(=10, 원소 마지막)이면 ☆++
3. 그렇지 않다면 ★++
4. 현재 ☆부터 ★까지의 부분합이 M과 같으면 결과++

![algorithm-towpointer_1]({{ site.baseurl }}/images/Algorithm/algorithm-towpointer_1.JPG)

위와 같은 과정을 ☆와 ★이 N까지 진행한다.

## 코드

```cpp
#include <bits/stdc++.h>
using namespace std;

int arr[10001];

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    int n, m;
    cin >> n >> m;

    for(int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    int s=0, e=0;
    int sum = 0;
    int ans = 0;
    while(1) {
        if(s != n) {
            if(sum >= m) {
                sum -= arr[e++];
            } else if(sum < m){
                sum += arr[s++];
            }
            if(sum == m) ans++;
        } else if(e != n) {
            sum -= arr[e++];
            if(sum == m) ans++;
        } else {
            break;
        }
    }
    cout << ans;
    return 0;
}
```

## 투 포인터의 시간복잡도
> 매 루프마다 항상 2개의 포인터 중 하나는 1씩 증가하고 있고, 각 포인터가 N번 누적 증가해야 알고리즘이 끝난다. 따라서 __투 포인터의 시간복잡도는 O(N)__ 이다.