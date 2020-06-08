---
title: "문자열 매칭 알고리즘"
category: Algorithm
date: 2019-08-03 15:17:59
comments: true
order: 24
---

## 목차
>> __1. 단순 문자열 매칭__ <br>
>> __2. KMP(Knuth-Morris-Pratt)__




<br>
<br>

## 단순 문자열 매칭 알고리즘
## 단순 문자열 매칭 알고리즘 이란?
* 특정한 글이 있을 때 그 글 안에서 하나의 __문자열을 찾는 알고리즘__ 이다.




## 단순 문자열 매칭 알고리즘의 특징
* __KMP 문자열 매칭 알고리즘__ 을 배우기 전에 먼저 단순 비교 문자열 매칭 알고리즘을 알고가는것이 좋다. 




## 단순 문자열 매칭 알고리즘 예시
* 단순 문자열 매칭 알고리즘은 단순히 문자열을 하나씩 확인하는 알고리즘이다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/simplestringmatching.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/simplestringmatching.JPG" title="Check out the image">
</a>


> * 이러한 방식을 이용하면 O(N * M)의 시간 복잡도를 가진다.




{% highlight javascript %}
#include <iostream>

using namespace std;

int findString(string parent, string pattern) {
	int parentSize = parent.size();
	int patternSize = pattern.size();
	for(int i = 0; i <= parentSize - patternSize; i++) {
		bool finded = true;
		for(int j = 0; j < patternSize; j++) {
			if(parent[i + j] != pattern[j]) {
				finded = false;
				break;
			}
		}
		if(finded) {
			return i;
		}
	}
	return -1;
}

int main(void) {
	string parent = "Hello World";
	string pattern = "llo W";
	int result = findString(parent, pattern);
	if(result == -1) {
		printf("not found");
	} else {
		printf("%d 번째에서 찾았습니다. ", result + 1);
	}
	return 0;
	
}
{% endhighlight %}





## KMP(Knuth-Morris-Pratt)
## KMP(Knuth-Morris-Pratt) 이란?
* 특정한 글이 있을 때 그 글 안에서 하나의 __문자열을 찾는 알고리즘__ 이다.
* 단순 문자열 매칭 경우 모든 경우를 비교하는 방법이고 KMP 알고리즘의 경우 __모든 경우를 다 비교하지 않아도 부분 문자열을 찾을 수 있는__ 알고리즘이다.




## KMP(Knuth-Morris-Pratt) 알고리즘의 특징
* __KMP 알고리즘__ 은 '반복되는 연산을 얼마나 줄일 수 있는지 판별' 하여 매칭할 문자열 찾고 불필요한 부분은 빠르게 넘어간다.
* __KMP 문자열 매칭 알고리즘__ 은 __접두사__ 와 __접미사__ 의 개념을 활용한다.
  + __aab__ bc __baa__ 문자열을 예시로 들면 아래와 같다.
  	- __접두사 :__ aab
  	- __접미사 :__ baa





## KMP(Knuth-Morris-Pratt) 알고리즘 예시

* KMP 알고리즘을 구현하기 위해서는 우선 __접두사와 접미사가 일치하는 최대 길이__ 를 구하는 것이다.
1. j번째의 패턴과 i번째의 패턴이 일치하는 경우 i, j 증가한다.
2. 일치하지 않는 경우 i 만 증가한다.
3. __일치하지 않는 경우 만약 j > 0 이라면 j 만 감소하고 일치 여부를 확인한다.__

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_maketable.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_maketable.JPG" title="Check out the image">
</a>

* 다음과 같이 수행이 되어지는데 주의 깊게 봐야할 부분은 Step 3~4, Step 8이다.
* __Step 3.__ j의 패턴과 i의 패턴이 일치 하지 않고 j > 0 이기 때문에 j만 감소시킨다.
* __Step 4.__ j를 감소시키고 패턴이 일치하는지 확인한다. 확인 결과 같지 않기 때문에 i만 증가시킨다.
* __Step 8.__ j와 i의 패턴이 동일하기 때문에 j + 1이 최대 일치 길이로 갱신 되는 것이다. 

* 접두사와 접미사가 일치하는 경우에 한해서 __점프(jump)__ 를 수행할 수 있다는 점에서 효율적이다.


* 접두사와 접미사 최대 일치 길이 테이블 구하기
{% highlight javascript %}
#include <iostream>
#include <vector>
using namespace std;

vector<int> makeTable(string pattern) {
	int patternSize = pattern.size();
	vector<int> table(patternSize, 0);
	int j = 0;
	for(int i = 1; i < patternSize; i++){
		while(j > 0 && pattern[i] != pattern[j]) {
			j = table[j - 1];
		}
		if(pattern[i] == pattern[j]) {
			table[i] = ++j;
		}
	}
	return table;
}

int main(void) {
	string pattern = "abacaaba";
	vector<int> table = makeTable(pattern);
	
	for(int i = 0; i < table.size(); i++){
		printf("%d ", table[i]);
	}
	return 0;
}
{% endhighlight %}





* parent와 pattern의 문자열을 하나씩 비교한다.
* Step 4에서 __parent[3] != pattern[3]: b != c__ 서로 다른 문자가 발견되면 일치하는 접두사 크기에 한해서만 부분 문자열의 인덱스를 Step 5와 같이 이동시킨다. 이후 계속적으로 하나씩 비교한다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_1.JPG" title="Check out the image">
</a>

* Step 9에서도 마찬가지로 서로 다른 문자가 발견되면 해당 최대 일치 길이에 맞게 이동시킨다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_2.JPG" title="Check out the image">
</a>

* Step 16과 같이 패턴과 일치하는 문자열을 발견했다. 아직 모든 문자열을 검사한것이 아니기 때문에 계속적으로 비교한다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_3.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_3.JPG" title="Check out the image">
</a>

* 하나의 패턴을 찾은(Step 16) 이후 접두사가 일치하는 한 최대 일치 길이만큼 이동시키고 계속적으로 비교한다.__(Step 18. parent[14]:c vs pattern[3]:c)__
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_4.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/kmp_4.JPG" title="Check out the image">
</a>





* 문자열 매칭
{% highlight javascript %}
#include <iostream>
#include <vector>
using namespace std;

vector<int> makeTable(string pattern) {
	int patternSize = pattern.size();
	vector<int> table(patternSize, 0);
	int j = 0;
	for(int i = 1; i < patternSize; i++){
		while(j > 0 && pattern[i] != pattern[j]) {
			j = table[j - 1];
		}
		if(pattern[i] == pattern[j]) {
			table[i] = ++j;
		}
	}
	return table;
}

void KMP(string parent, string pattern) {
	vector<int> table = makeTable(pattern);
	int parentSize = parent.size();
	int patternSize = pattern.size();
	int j = 0;
	for(int i = 0; i < parentSize; i++) {
		while( j > 0 && parent[i] != pattern[j]) {
			j = table[j - 1];
		}
		if(parent[i] == pattern[j]) {
			if( j == patternSize - 1) {
				printf("%d번째에서 찾았습니다. \n", i - patternSize + 2);
				j = table[j];
			} else {
				j++;
			}
		}
	}
	
}

int main(void) {
	string parent = "ababacabacaabacaaba";
	string pattern = "abacaaba";
	KMP(parent, pattern);
	return 0;
}
{% endhighlight %}





## 관련된 Post





## References
> * <a href="https://bowbowbow.tistory.com/6">https://bowbowbow.tistory.com/6<a>
> * <a href="https://www.zerocho.com/category/Algorithm/post/5893405b588acb00186d39e0">https://www.zerocho.com/category/Algorithm/post/5893405b588acb00186d39e0<a>
> * <a href="https://www.youtube.com/watch?v=WAzjfl7Pt_4&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=33">33강 - 단순 문자열 매칭 알고리즘<a>
> * <a href="https://blog.naver.com/ndb796/221240660061">https://blog.naver.com/ndb796/221240660061<a>