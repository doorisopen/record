---
title: "에라토스테네스의 체"
category: Algorithm
date: 2019-07-29 17:32:30
comments: true
order: 15
---

## 에라토스테네스의 체 란?
* __소수(Prime Number) 판별 알고리즘이다.__
  * 소수란, __'양의 약수를 두개만 가지는 자연수'__ 를 의미 2, 3, 5, 7, ...등이 존재한다. 즉, 1과 자기자신을 약수로 가지는 수를 말한다.
* __대량의 소수를 한꺼번에 판별__ 하고자 할 때 사용하는 알고리즘이다.

## 에라토스테네스의 체의 특징
* 소수를 대량으로 빠르고 정확하게 구할 수 있다.

## 에라토스테네스의 체의 알고리즘 예시

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/Sieve_of_Eratosthenes_animation.gif" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/Sieve_of_Eratosthenes_animation.gif" title="Check out the image">
</a>


## 일반적인 소수 판별 코드
* __시간복잡도 O(N) 만큼 걸린다.__
* 모든 경우의 수를 다 돌며 약수 여부를 확인한다는 점에서 __비효율적이다.__

{% highlight javascript %}
#include <stdio.h>

bool PrimeNumber(int x){
	for(int i = 2; i < x; i++){
		if(x % i == 0) return false;
	}
	return true;
}
int main(void){
	printf("%d",PrimeNumber(97));
	return 0;
}
{% endhighlight %}




## 제곱근을 이용한 소수 판별 코드
* __시간복잡도 O(N^(1/2)) 만큼 걸린다.__
* 제곱근 까지만 약수의 여부를 확인한다.
* 약수들은 대칭을 이룬다 ex) 8 : 2 * 4 = 4 * 2

{% highlight javascript %}
#include <stdio.h>
#include <math.h>

bool isPrimeNumber(int x) {
	// 제곱근을 확인하는 함수 
	int end = (int) sqrt(x);
	for(int i = 2; i <= end; i++){
		if( x % i == 0) return false;
	}
	return true;
}

int main(void){
	printf("%d",isPrimeNumber(17));
	return 0;
}
{% endhighlight %}




## 에라토스테네스의 체 C 구현

{% highlight javascript %}
#include <stdio.h>
#include <math.h>

int num = 100000;
int a[100001];

void PrimeNumberSieve() {
	// 제곱근을 확인하는 함수 
	// 초기화 
	for(int i = 2; i <= num; i++){
		a[i] = i;
	} 
	
	for(int i = 2; i <= num; i++){
		if(a[i] == 0) continue;
		for(int j = i + i; j <= num; j += i ){
			a[j] = 0;
		}
	}
	for(int i = 2; i <= num; i++){
		if(a[i]!=0) printf("%d ", a[i]);
	}
}

int main(void){
	PrimeNumberSieve();
	return 0;
}
{% endhighlight %}

## 관련된 Post


## References

> * <a href="https://blog.naver.com/ndb796/221233595886">22. 에라토스테네스의 체<a>
> * <a href="https://ko.wikipedia.org/wiki/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98_%EC%B2%B4">위키 - 에라토스테네스의 체<a>