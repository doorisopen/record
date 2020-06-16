---
title: "sort()"
category: Cpp
date: 2019-07-15 13:32:30
comments: true
order: 1
---

## sort() 함수의 기본 사용법1
* sort() 함수는 C++의 algorithm 헤더에 포함되어있다. 사용법은 아래와 같다.

{% highlight javascript %}
#include <algorithm>
#include <iostream>
using namespace std;

int main(void){
	int a[10] = {9, 3, 2, 5, 4, 6, 7, 8, 1, 10};
	sort(a, a+10);
	for(int i = 0; i < 10; i++){
		cout << a[i] << ' ';
	}
	return 0;
}
{% endhighlight %}
 
 sort() 함수를 살펴보면 배열의 시작점 주소와 마지막 주소를 넣어줬음을 볼 수 있다. 위의 함수를 실행하고 나면 오름차순으로 정렬된 배열을 출력하는것을 볼 수 있다.

* sort()함수는 기본적으로 오름차순 정렬이 기본이다.


## sort() 함수의 기본 사용법2
* sort()함수는 정렬 기준을 정해서 정렬할 수 있다.

{% highlight javascript %}
#include <algorithm>
#include <iostream>
using namespace std;

bool compare(int a, int b){
	return a > b;
}

int main(void){
	int a[10] = {9, 3, 2, 5, 4, 6, 7, 8, 1, 10};
	sort(a, a+10, compare);
	for(int i = 0; i < 10; i++){
		cout << a[i] << ' ';
	}
	return 0;
}
{% endhighlight %}

sort() 함수를 살펴보면 sort()의 세 번째 인자 값으로 compare()함수를 만들어서 넣어줬음을 볼 수 있다. 위와 같이 정렬의 기준을 자신이 원하는 형태로 설정할 수 있다.

* 소스코드의 실행 결과는 내림차순으로 출력된다.


## sort() 함수의 기본 사용법3
* 위의 1, 2 의 경우에는 단순 데이터 정렬 기법이다. 또한 실무에서 사용하기에 적합하지 않다. 실무에서 프로그래밍을 할 때는 모든 데이터들이 개겣로 정리되어 내부적으로 여러개의 변수를 포함하고 있기 때문이다. 이 경우 가장 중요한 정렬 방식은 __'특정한 변수를 기준으로'__ 정렬하는 것이다.

{% highlight javascript %}
#include <algorithm>
#include <iostream>
using namespace std;

class Student {
	public:
		string name;
		int score;
		Student(string name, int score){
			this->name = name;
			this->score = score;
		}
		// 정렬 기준 점수가 낮은 순서 
		bool operator < (Student &student){
			return this->score < student.score;
		}
};

int main(void){
	Student students[] = {
		Student("김길동", 90),
		Student("이길동", 10),
		Student("박길동", 30),
		Student("나길동", 80)
	};
	
	sort(students, students + 4);
	
	for(int i=0; i<4; i++){
		cout << students[i].name << ' ';
	} 
	return 0;
}
{% endhighlight %}

* bool operator < (Student &student)
  + [연산자 오버로딩 참고](https://mufflemumble.tistory.com/27)
   
* 위 소스코드는 점수를 기준으로 학생을 정렬해서 이름을 출력하는 코드이다.


## sort() 함수의 기본 사용법4
__변수가 2개일 때 '한 개'의 변수를 기준으로 정렬하는 방법__
* 일반적으로 클래스를 정의하는 방식은 프로그래밍 속도 측면에서 유리하지 않다. 클래스를 이용하는 방식은 실무에 적합한 방식이며, 일반적으로 프로그래밍 대회에서는 Pair 라이브러리를 사용하는 것이 유리하다.

{% highlight javascript %}
#include <algorithm>
#include <iostream>
#include <vector> 
using namespace std;

int main(void){
	vector<pair<int, string> > v;
	v.push_back(pair<int, string>(70, "김길동"));
	v.push_back(pair<int, string>(80, "유길동"));
	v.push_back(pair<int, string>(75, "박길동"));
	v.push_back(pair<int, string>(90, "이길동"));
	v.push_back(pair<int, string>(67, "학길동"));
	
	
	sort(v.begin(), v.end());
	
	for(int i = 0; i < v.size(); i++){
		cout << v[i].second << ' ';
	} 
	return 0;
}
{% endhighlight %}
 위 소스코드는 벡터(Vector) 라이브러리와 페어(Pair) 라이브러리를 이용해 클래스를 정의했던 방식을 변경한것이다. 이렇게 소스코드의 길이를 짧게 해주는 기법을 숏코딩(Short Coding)이라고 한다. 작성한 소스코드의 시간 복잡도가 동일하다면, 프로그래밍 대회에서는 소스코드가 짧을 수록 남들보다 앞설수 있다.


* __백터(Vector) STL은__ 배열과 같이 작동한다. 원소를 삽입(Push), 삭제(Pop)을 할 수 있다. 기존의 배열을 보다 사용하기 쉽게 개편한 자료구조다.
* __페어(Pair) STL은__ 한 쌍의 데이터를 처리할 수 있도록 해주는 자료구조다.

* v[].second에서 second는 배열의 2번째 값을 가리킨다는 의미이다.
* 위 소스코드는 점수를 기준으로 학생을 정렬해서 이름을 출력하는 코드이다.


## sort() 함수의 기본 사용법4-1

* 내림차순(점수 기준)으로 정렬하고 만약 동일 점수가 있다면 이름 사전 순서로 정렬한다.

```
#include <bits/stdc++.h>
using namespace std;

bool compare (pair<int, string> a, 
          pair<int, string> b){
    if(a.first == b.first) {
        return a.second < b.second; 
    } else {
        return a.first > b.first;
    }
}

int main(void) {

	vector<pair<int, string> > v;
	v.push_back(make_pair(1000, "a"));
	v.push_back(make_pair(300, "b"));
	v.push_back(make_pair(700, "c"));
	v.push_back(make_pair(250, "e"));
    v.push_back(make_pair(300, "g"));
	
	sort(v.begin(), v.end() ,compare);

    for(int i = 0; i < v.size(); i++) {
        cout << v[i].second << " " << v[i].first << "\n"; 
    }
	/**==Result==
		a 1000
		c 700
		b 300
		g 300
		e 250
	*/
	return 0;
}
```


## sort() 함수의 기본 사용법5
__변수가 3개일 때 '두 개'의 변수를 기준으로 정렬하는 방법__


{% highlight javascript %}
#include <algorithm>
#include <iostream>
#include <vector> 
using namespace std;

bool compare(pair<string, pair<int, int> > a,
	pair<string, pair<int, int> > b){
		if(a.second.first == b.second.first){
			return a.second.second > b.second.second;
		}else{
			return a.second.first > b.second.first;
		}
	}

int main(void){
	vector<pair<string, pair<int, int> > > v;
	v.push_back(pair<string, pair<int, int> >("김길동", pair<int, int>(90, 20150121)));
	v.push_back(pair<string, pair<int, int> >("유길동", pair<int, int>(97, 20130521)));
	v.push_back(pair<string, pair<int, int> >("박길동", pair<int, int>(40, 20050217)));
	v.push_back(pair<string, pair<int, int> >("이길동", pair<int, int>(60, 19900311)));
	v.push_back(pair<string, pair<int, int> >("학길동", pair<int, int>(20, 20111221)));
	
	
	sort(v.begin(), v.end(), compare);
	
	for(int i = 0; i < v.size(); i++){
		cout << v[i].first << ' ';
	} 
	return 0;
}
{% endhighlight %}


* __결론__
sort() 함수는 세 개의 변수까지만 자유자재로 다룰 수 있는 정도면 실무에서나 혹은 프로그래밍 대회에서 정렬 문제로 골머리 앓는 경우는 없을 것이라고 생각한다.

## References
* [연산자 오버로딩](https://mufflemumble.tistory.com/27)
* [8. C++ STL sort()함수 다루기1](https://blog.naver.com/ndb796/221227975229)
* [9. C++ STL sort()함수 다루기2](https://blog.naver.com/ndb796/221228004692)