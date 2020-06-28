---
title: "재귀(recursive)"
category: Algorithm
date: 2020-06-28 01:34:59
comments: true
order: 26
---

## 재귀란?
* 하나의 함수에서 자기 자신을 다시 호출해 작업을 수행하는 알고리즘
* 절차지향적사고, __귀납적사고__
  + 1번 도미노가 쓰러진다.
  + k번 도미노가 쓰러지면서 k+1번 도미노도 쓰러진다.

## 재귀 함수의 조건
* 특정 입력에 대해서는 자기 자신을 호출하지 않고 종료되어야함(base condition)
* 모든 입력은 base condition으로 수렴해야 함

{% highlight javascript %}
void func1(int n) {
    if(n==0) return; // base condition
    cout << n << ' ';
    func1(n-1);
}
{% endhighlight %}

## 재귀 활용 팁1
* 함수의 인자로 어떤 것을 받고 어디까지 계산한 후 자기 자신에게 넘겨줄지 명확하게 정해야 한다.
* 모든 재귀 함수는 반복문만으로 동일한 동작을 하는 함수를 만들 수 있다.
* 재귀는 반복문으로 구현했을 때에 비해 코드가 간결하지만 메모리/시간에서는 손해를 본다.

## 재귀 활용 팁2
* 한 함수가 __자기 자신을 여러 번 호출하게 되면 비효율적__ 일 수 있다.
{% highlight javascript %}
void fibo(int n) {
    if(n<=1) return 1; // base condition
    return fibo(n-1) + fibo(n-2);
}
{% endhighlight %}
> 위 코드의 시간복잡도는 O(1.618^n)으로 비효율적인 코드이다. 이유는 반복된 계산이 여러번 발생하기 때문이다.

## 재귀 활용 팁3
* 재귀함수가 자기 자신을 부를 때 __스택 영역__ 에 계속 누적이 된다.

## 재귀 연습문제
* 거듭제곱(a^b mod m)
* [BOJ 1629 곱셈](https://www.acmicpc.net/problem/1629)
* [BOJ 11729 하노이 탑 이동 순서](https://www.acmicpc.net/problem/11729)

#### 연습 문제1 - 거듭제곱(a^b mod m)
* a b m 3개의 정수가 연속으로 주어질때 a^b mod m 를 구하라.

<details><summary>연습 문제1-코드확인</summary>

{% highlight javascript %}
using ll = long long;
ll func1(ll a, ll b, ll m) {
    ll val = 1;
    while(b--) val = val * a % m;
    return val;
}
{% endhighlight %}

long long 타입을 하지 않는다면 int overflow가 발생할 수 있다.

따라서, a^b mod m 은 O(b)에 구할 수 있다.

</details>

<hr/>

#### 연습 문제2 - BOJ 1629 곱셈
* Hint
  + aⁿ * aⁿ = a²ⁿ
  + ex) 12^58 ≡ 4(mod 67) -> 12^116 ≡ 16(mod 67)
<details><summary>연습 문제2-코드확인</summary>

{% highlight javascript %}
#include <iostream>
using namespace std;
using ll = long long;

ll pow(ll a, ll b, ll c) {
    if(b==1) return a % c; // base condition
    ll val = pow(a, b/2, c);
    val = val * val % c;
    if(b%2 == 0) return val;
    return val * a % c;
}
int main(void) {
    ll a,b,c;
    cin >> a >> b >> c;
    cout << pow(a,b,c);
    return 0;
}
{% endhighlight %}

시간복잡도는 b가 절반씩 깎이기 때문에 O(logb) 이다.

</details>

<hr/>

#### 연습 문제3 - BOJ 11729 하노이 탑 이동 순서
* Hint
  + n-1개의 원판을 기둥 1에서 기둥 2로 옮긴다.
  + n번 원판을 기둥 1에서 기둥 3으로 옮긴다.
  + n-1개의 원판을 기둥 2에서 기둥 3으로 옮긴다.
  + __->원판이 n-1개일 때 옮길 수 있으면 원판이 n개일 때에도 옮길 수 있다.__
  
<details><summary>연습 문제3-코드확인</summary>

{% highlight javascript %}
#include <bits/stdc++.h>
using namespace std;
typedef pair<int,int> pi;

queue<pi> q;
int n;

void hanoi(int num, int from, int tmp, int to) {
    if(num == 1) {
        q.push({from, to});
    }
    else {
        hanoi(num-1, from, to, tmp);
        q.push({from, to});
        hanoi(num-1, tmp, from, to);
    }
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    cin >> n;
    hanoi(n,1,2,3);

    cout << q.size() << "\n";
    while (!q.empty()) {
        cout << q.front().first << " " << q.front().second << "\n";
        q.pop();
    }
    return 0;
}
{% endhighlight %}

</details>

<hr/>

#### 연습 문제4 - BOJ 1074 Z
  
<details><summary>연습 문제4-코드확인</summary>

{% highlight javascript %}
#include <stdio.h>
#include <math.h>
using namespace std;
int N, r, c;
int cnt = 0;

void Z(int x, int y, int len) {
	if(x == r && y == c) {
		printf("%d", cnt);
		return ;	
	} 
	if(r < x + len && r >= x && c < y + len && c >= y) {
		Z(x ,y ,len/2);
		Z(x ,y + len/2 ,len/2);
		Z(x + len/2 ,y ,len/2);
		Z(x + len/2 ,y + len/2 ,len/2);
	} else {
		cnt += len * len;
	}
}
int main(void) {

	scanf("%d %d %d", &N, &r, &c);
	Z(0, 0, pow(2,N));
	return 0;
}
{% endhighlight %}

</details>

<hr/>

## References
* [[바킹독의 실전 알고리즘] 0x0B강 - 재귀](https://www.youtube.com/watch?v=8vDDJm5EewM)