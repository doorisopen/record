---
title: "다이나믹 프로그래밍(Dynamic Programming)"
category: Algorithm
date: 2019-07-26 16:40:30
comments: true
order: 16
---

## 다이나믹 프로그래밍(Dynamic Programming) 이란?
* __다이나믹 프로그래밍이__ 란 __'하나의 문제는 단 한 번만 풀도록 하는 알고리즘'__ 이다.
  + 한 번 푼 것을 여러 번 다시 푸는 비효율적인 알고리즘을 개선시키는 방법이기도 하다.
* 큰 문제를 풀기위해 작은 문제를 풀어 큰 문제를 풀어가는 방법이다.
* __다이나믹 프로그래밍(Dynamic Programming)__ 은 '동적 프로그래밍', '동적 계획법' 이라고도 한다. 


## 다이나믹 프로그래밍(Dynamic Programming)의 특징
* 다이나밍 프로그래밍은 다음의 가정 하에 사용할 수 있다.
  1. 큰 문제를 작은 문제로 나눌 수 있다.
  2. 작은 문제에서 구한 정답은 그것을 포함하는 큰 문제에서도 동일하다. 
* D[i] = D[i - 1]  + D[i - 2] 처럼 인접한 수의 관계를 이용해 문제를 푸는것을 점화식을 이용한 풀이 라고도 한다. 많은 동적 계획법 문제가 이 점화식을 구하면 답을 구할 수 있다. 


## 다이나믹 프로그래밍(Dynamic Programming)의 알고리즘 예시

* 대표적인 DP 문제로 피보나치 수열을 들 수 있다.
* 피보나치 함수를 푸는방법으로 __재귀함수를__ 이용한 방법이 있지만 느리다. 또한 수가 커질수록 엄청난 스택이 쌓인다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/dp_fib.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/dp_fib.JPG" title="Check out the image">
</a>

* __피보나치 수열의 점화식 :__ D[i] = D[i - 1]  + D[i - 2]
* 위의 사진처럼 7을 피보나치 함수에 넣으면 사진과 같이 호출되어진다. 그러나 특정 부분에서 동일한 값의 피보나치 함수가 호출 됨을 볼 수 가있다. 이것을 불필요하게 한 번 푼 것을 여러 번 다시 푸는 비효율적인 방법이라고 할 수 있다. 
* 이것을 해결하기 위해서 __'메모이제이션'__ 이라는 방법을 이용하면 엄청난 시간 단축 효과를 볼 수 있다.
  + __메모이제이션 이란,__ 한 번 구한 값을 다시 구하는 일이 없도록 하기 위한 방법이다.

## 1. Dynamic Programming 기본 형태 구현
{% highlight javascript %}
#include <stdio.h>

int d[100];
// dp 의 기본 형태 
int dp(int x){
	if( x == 1 ) return 1;
	if( x == 2 ) return 1;
	return dp(x-1) + dp(x-2);
}
int main(void){
	printf("%d ",dp(40));
	return 0;
}
{% endhighlight %}


## 2. Dynamic Programming 메모이제이션(Memoization)이용하여 구현
{% highlight javascript %}
#include <stdio.h>

int d[100];

// 개선한 dp 형태 
int dp(int x){
	if( x == 1 ) return 1;
	if( x == 2 ) return 1;
    // 메모이제이션 이용
	if(d[x] != 0) return d[x];
	return d[x] = dp(x-1) + dp(x-2);
}

int main(void){
	printf("%d ",dp(40));
	return 0;
}
{% endhighlight %}


## 관련 문제
> * <a href="https://github.com/doorisopen/ProblemSolving/blob/master/BOJ/boj11726.cpp">[BOJ]11726 - 2*n 타일링<a>
> * <a href="https://github.com/doorisopen/ProblemSolving/blob/master/BOJ/boj11727.cpp">[BOJ]11727 - 2*n 타일링2<a>
> * <a href="https://github.com/doorisopen/ProblemSolving/blob/master/BOJ/boj2133.cpp">[BOJ]2133 - 타일 채우기<a>
> * <a href="https://github.com/doorisopen/ProblemSolving/blob/master/BOJ/boj14852.cpp">[BOJ]14852 - 타일 채우기3<a>


## References
> * <a href="https://dpdpwl.tistory.com/57">[Algorithm]동적계획법(다이나믹 프로그래밍)<a>
> * <a href="https://blog.naver.com/ndb796/221233570962">20강 - 다이나믹 프로그래밍(Dynamic Programming)<a>
> * <a href="https://www.youtube.com/watch?v=FmXZG7D8nS4">21강 - 다이나믹 프로그래밍(Dynamic Programming)<a>