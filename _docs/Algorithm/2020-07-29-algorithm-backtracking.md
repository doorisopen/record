---
title: "백트래킹(backtracking)"
category: Algorithm
date: 2020-07-29 01:34:59
comments: true
order: 28
---

## 목차
* 백트래킹 이란?
* 연습문제1 - BOJ 15649 N과 M(1)
* 연습문제2 - BOJ 9663 N-Queen

## 백트래킹(backtracking) 이란?
현재 상태에서 가능한 모든 후보군을 따라 들어가며 탐색하는 알고리즘


## 연습문제1 - BOJ 15649 N과M(1)
* [문제 링크](https://www.acmicpc.net/problem/15649)

원소가 1,2,3,4 총 4개가 주어져 있습니다. 

이때 첫 번쨰 자리가 1인 경우 두 번째 자리에 올 수 있는 원소는 2,3,4 입니다.

두 번째 자리에 2가 왔을때 세 번째 자리에 올 수 있는 원소는 3, 4 입니다.

즉, 원소 1~4가 주어질때 3자리 순열을 구하는 문제입니다. 

순열과 조합을 자세히 알고싶다면 [이곳](https://doorisopen.github.io/developers-library/Algorithm/2020-07-22-algorithm-permutation-combination/)을 참고해주세요.

<details><summary>N과M(1)-코드확인</summary>

{% highlight c++ %}
// 15649 - N과 M(1)
#include <stdio.h>
#include <vector>
#define MAX 9

using namespace std;
int N, M;
bool select[MAX];
vector<int> v;

void print() {
	for(int i = 0; i < v.size(); i++) {
		printf("%d ",v[i]);
	}
	printf("\n");
}

void dfs(int cnt) {
	if(cnt == M) { // M개의 조합이 되었을때 출력한다 
		print();
		return;
	}
	for(int i = 1; i <= N; i++) {
		if(select[i] == true) continue;
		select[i] = true;
		v.push_back(i);
		dfs(cnt+1);
		select[i] = false; // (1)
		v.pop_back(); // (2)
	}
}
int main(void) {
	scanf("%d %d",&N ,&M);
	dfs(0);
	return 0;
} 
{% endhighlight %}


* `(1)과 (2)`
  + 위와 같이 __상태 값을 이전으로 되돌려서__ 모든 경우의 수를 탐색합니다.

</details>

<hr/>

## 연습문제2 - BOJ 9663 N-Queen
* [문제 링크](https://www.acmicpc.net/problem/9663)

<details><summary>N-Queen-코드확인</summary>

{% highlight c++ %}
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define MAX 16

int n, result = 0;
int m[MAX];

bool isPossible(int c) {
    for (int i = 1; i < c; i++)
    {
        //같은 행에 있는지 여부
        if(m[i] == m[c]) {
            return false;
        }
        //대각선 검사
        if(abs(m[i] - m[c]) == abs(i - c)) {
            return false;
        }
    }
    return true;
}

void dfs(int d) {
    if(d > n) {
        ++result;
        return;
    } else {
        //d열 i행 탐색
        for (int i = 1; i <= n; i++)
        {
            m[d] = i;
            // 노드 확인
            if(isPossible(d)) {
                dfs(d+1);
            } else {
                //백트래킹
                m[d] = 0;
            }
        }
    }
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);

    cin >> n;
    for (int i = 1; i <= n; i++)
    {
        fill(m, m+n, 0);
        m[1] = i;
        dfs(2);
    }
    cout << result;
    return 0;
}
{% endhighlight %}

</details>

<hr/>

## References
* [바킹독의 실전 알고리즘](https://www.youtube.com/watch?v=Enz2csssTCs)