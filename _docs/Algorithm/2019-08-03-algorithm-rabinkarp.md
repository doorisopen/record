---
title: "라빈 카프(Rabin-Karp) 알고리즘"
category: Algorithm
date: 2019-08-03 19:50:59
comments: true
order: 23
---


## 라빈 카프(Rabin-Karp) 알고리즘 이란?
* __라빈 카프(Rabin-Karp)__ 알고리즘은 문자열의 해시 값을 비교하여 그 일치 여부를 검사하는 알고리즘이다.


## 라빈 카프(Rabin-Karp) 알고리즘의 특징
* __라빈 카프(Rabin-Karp)__ 알고리즘은 항상 빠르지는 않지만 일반적인 경우 빠르게 작동하는 간단한 구조의 문자열 매칭 알고리즘이라는 점에서 자주 사용된다.
* __라빈 카프(Rabin-Karp)__ 알고리즘은 해시 기법을 사용한다.
  + __해시란,__ 일반적으로 긴 데이터가 있다면 그것을 상징하는 짧은 데이터로 바꾸어주는 기법이다.
  + 상징하는 데이터로 바꾸어 처리한다는 점에서 단순 해시 알고리즘의 연산 속도는 O(1)로 속도가 빠르다는 장점을 가지고 있다.
* __라빈 카프(Rabin-Karp)__ 알고리즘은 여러 개의 문자열을 비교할 때 항상 해시 값을 구하여 비교하고, 해시 값은 거의 일치하는 일이 없기 때문에 __'parent'__ 와 찾으려는 __'pattern' 문자열__ 의 해시 값이 일치하는 경우에만 문자열을 재검사 하여 정확히 일치하는지 확인하는 알고리즘이다. 

* __라빈 카프 알고리즘이 속도가 빠른 이유__
  + parent 문자열 해시를 구하는 연속적인 과정이 연결된 수학적 수식에 의해 반복되기 때문이다.

* __참고__

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/rabinkarp_hash.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/rabinkarp_hash.JPG" title="Check out the image">
</a>

* 위와 같이 문자열이 달라지면 결과로 출력되는 해시 값도 바뀌는 것을 알 수 있다.
+ __충돌(Collision) :__ 해시 값이 중복되는 경우를 말한다.
  - 출돌이 발생하는 경우 포인터(Pointer)를 이용해 연결 자료 구조를 이용해 해결한다.




## 라빈 카프(Rabin-Karp) 알고리즘 예시
* __핵심__

Step 1에서 2로 한칸 이동하면서 가장 바로 앞의 'a'만큼의 수치를 빼 준 뒤에 2를 곱하고 새롭게 뒤에 들어온 'a'의 수치를 더해준다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/rabinkarp_1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/rabinkarp_1.JPG" title="Check out the image">
</a>

> 따라서, __parent 해시 값 = 2 * ( parent 해시 값 - 가장 앞에 있는 문자의 수치) + 새롭게 들어온 문자의 수치__

## Source Code
{% highlight javascript %}
#include <iostream>

using namespace std; 

void findString(string parent, string pattern) {
	int parentSize = parent.size();
	int patternSize = pattern.size();
	int parentHash = 0, patternHash = 0, power = 1; // power: 제곱수 
	for(int i = 0; i <= parentSize - patternSize; i++) {
		if(i == 0) {
			for(int j = 0; j < patternSize; j++) {
				parentHash += parent[patternSize - 1 - j] * power;
				patternHash += pattern[patternSize - 1 - j] * power;
				if(j < patternSize - 1) power *= 2;
			}
		} else {
			parentHash = 2 * (parentHash - parent[i - 1] * power) + parent[patternSize - 1 + i];
			
		}
		if(parentHash == patternHash) {
			bool finded = true;
			for(int j = 0; j < patternSize; j++) {
				if(parent[i + j] != pattern[j]) {
					finded = false;
					break;
				}
			}
			if(finded) {
				printf("%d번째에서 발견했습니다. \n", i + 1);
			}
		}
	}
	
}

int main(void) {
	string parent = "ababacabacaabacaaba";
	string pattern = "abacaaba";
	findString(parent, pattern);
	return 0;
}
{% endhighlight %}

* 정확하게 __해시 값의 일치 여부를 검증__ 하기 위해서 __나머지 연산(MOD)__ 를 사용하는 경우가 많다. 하지만 일반적으로 __오버 플로우(Over Flow)__ 가 발생해도 해시 값 자체는 동일하기 때문에 나머지 연산을 굳이 사용해주지 않아도 된다. 그러나 더 안정적인 프로그램을 작성하고자 할 때는 __나머지 연산__ 을 해주는 것이 좋다.


## References
> * <a href="https://www.youtube.com/watch?v=kJJQJDsjXc8&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=35">35강 - 라빈 카프(Rabin-Karp) 문자열 매칭 알고리즘<a>
> * <a href="https://blog.naver.com/ndb796/221240679247">https://blog.naver.com/ndb796/221240679247<a>