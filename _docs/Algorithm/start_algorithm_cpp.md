---
title: C++로 알고리즘 시작하기
category: Algorithm
date:   2020-06-08 00:30:59
comments: true
order: 1
---

# 알고리즘 시작하기 
* PS, 코딩테스트를 위해 알고리즘을 공부하기 전에 알아야할 __기초 내용 그리고 팁__

> 1. 알고리즘 종류, 자료구조 종류
> 2. 시간 복잡도, 공간 복잡도, 자료형
> 3. C++ STL, 표준 입출력

<br/>
<br/>
<br/>

# 알고리즘 종류
* BFS
* DFS
* 재귀
* 백트래킹
* 시뮬레이션
* 정렬
* 다이나믹 프로그래밍
* 그리디
* 이분탐색
* 이진 검색 트리
* KMP 알고리즘
* 라빈 카프 알고리즘
* 위상정렬
* 크루스칼 알고리즘
* 프림 알고리즘
* 플로이드 와샬 알고리즘
* 다익스트라 알고리즘
* 슬라이딩 윈도우(투 포인터)
* 수학
* etc..

# 자료구조 종류
* 배열
* 연결 리스트
* 스택
* 큐
* 덱
* 그래프
* 트리
* 힙
* 해시
* etc..

# 시간, 공간복잡도
## 시간복잡도(Time Complexity)
* 입력의 크기와 문제를 해결하는데 걸리는 시간의 상관관계
* 시간복잡도는 프로그램 수행시간을 분석하는 것이고 반복문에 크게 영향을 받는다.

## 빅오표기법(Big-O Notaion)
주어진 식을 값이 가장 큰 대표항만 남겨서 나타내는 방법

* __O(N):__ 5N+3, 2N+10lgN, 10N
* __O(N^2):__ N^2 + 2N+5, 6N^2 + 20N + 10lgN
* __O(NlgN):__ NlgN + 30N + 10, 5lgN + 6
* __O(1):__ 5, 16, 36

> __O(1) < O(lgN) < O(N) < O(NlgN) < O(N^2) < O(2^N) < O(N!)__

|  <center>N의 크기</center> |  <center>허용 시간복잡도</center> |
|:--------:|:--------:|
| N <= 11 | O(N!) |
| N <= 25 | O(2^N) |
| N <= 100 | O(N^4) |
| N <= 500 | O(N^3) |
| N <= 3,000 | O(N^2lgN) |
| N <= 5,000 | O(N^2) |
| N <= 1,000,000 | O(NlgN) |
| N <= 10,000,000 | O(N) |
| 그 이상 | O(lgN), O(1) |


```cpp
// 1. O(log(n))
while(n)
	n/=2;

// 2. O(sqrt(n))
for(int i=0;i*i<=n;i++);

// 3. O(n)
for(int i=0;i<n;i++);

// 4. O(nlog(n))
for(int i=0;i<n;i++)
	for(int j=i;j;j/=2);

// 5. O(nsqrt(n))
for(int i=0;i<n;i++)
	for(int j=0;j*j<=i;j++);

// 6. O(n^2)
for(int i=0;i<n;i++)
	for(int j=0;j<n;j++);

// 7. O(n^3)
for(int i=0;i<n;i++)
	for(int j=0;j<n;j++)
		for(int k=0;k<n;k++);

// 8. O(2^n)
int function(int n){
	if(n==0||n==1)
		return 1;
	return function(n-1)+function(n-2);
}

// 9. O(n!)
// n개의 데이터를 나열하는 방법의 수
void function(int x, vector<int> pick, vector<bool> picked){
	if(x==n){
		for(int i=0;i<pick.size();i++)
			printf("%d\n", pick[i]);
		return;
	}
	for(int i=0;i<n;i++){
		if(picked[i])
			continue;
		pick.push_back(i);
		picked[i]=true;
		function(x+1, pick, picked);
		pick.pop_back();
		picked[i]=false;
	}
	return;
}
```

## 공간복잡도
* 입력의 크기와 문제를 해결하는데 필요한 공간의 상관관계
* 공간복잡도는 프로그램의 메모리 사용량을 분석하는 것이다.
  + 간단하게 사용한 배열의 크기 * (해당 자료형의 크기) 로 계산한다. 
  + 보통 기타 지역변수나 헤더파일, 함수, 등을 생각해서 5~10MB 정도는 여유로 빼고 생각한다.
* 512MB = 1.2억개의 int(4byte)

# 정수, 실수 자료형
## 정수 자료형
* 하나의 char (1byte == 8bit)

* __unsigned char:__ __2^7__ 2^6 2^5 2^4 2^3 2^2 2^1 2^0
  + 자료형의 범위 = __0__ ~ 2^0+2^1...+2^7(=2^8 - 1 =__255__)
  + 0 0 0 0 1 0 0 1 = 9
  + 1 0 0 0 0 0 1 1 = 131
* __char:__ __-2^7__ 2^6 2^5 2^4 2^3 2^2 2^1 2^0
  + __2's complement__
  + 자료형의 범위 = -2^7(=__-128__) ~ 2^0+2^1...+2^6(2^7 - 1=__127__)
  + 0 0 0 0 1 0 0 1 = 9
  + 1 0 0 0 0 0 1 1 = -125

* __short__ (2 byte) 2^15-1(=32767)
* __int__ (4 byte) 2^31-1(=2.1*10^9)
* __long long__ (8 byte) 2^63-1(=9.2*10^18)
  + 80번째 피보나치 구할때는 이 자료형을 사용..

## 실수 자료형
* __float__ (4 byte)
  + 실수를 나타낼때 아래와 같이 나타낸다.
  + sign(1, 부호) exponent(8, 지수) fraction(23, 유효숫자)
* __double__ (8 byte)
  + sign(1) exponent(11) fraction(52)

* __3.75를 이진수로__
  + 2 + 1 + 0.5 + 0.25 = 2^1 + 2^0 + 2^-1 + 2^-2 = 11.11(2)


* __실수를 저장하는 방식:__ IEEE-754 format
* 실수의 저장/연산 과정에서 반드시 오차가 발생할 수 밖에 없다.
  + 0.1 + 0.1 + 0.1 == 0.3 -> != 틀리다!! 
  + 이유는 0.1은 이진수로 나타내면 무한소수여서 애초에 오차가 있는 채로 저장되기 때문이다.
  + float: 유효숫자 6자리(10^-6)까지 안전
  + double: 유효숫자 15자리(10^-15)까지 안전
    - __1-10^15 ~ 1+10^15 사이의 값__ 은 어느정도 보장된다는 말임
* double에 long long 범위의 정수를 함부로 담으면 안된다.
* 실수를 비교할 때는 등호를 사용하면 안된다.

## 연습 코드

```cpp
#include <iostream>

using namespace std;

// O(n)
int func1(int num) {
	int sum = 0;
	for(int i = 1; i <= num; i++) {
		if(i % 3 == 0 || i % 5 == 0) {
			sum += i;
		}
	}
	return sum;
}
// O(N^2)
int func2(int *arr, int num) {
	int sum = 0;
	for(int i = 0; i < num; i++) {
		for(int j = i+1; j < num; j++) {
			if(arr[i] + arr[j] == 100) return 1;
		}
	}
	return 0; 
}
// O(n^1/2)
int func3(int N) {
	for(int i = 1; i*i <= N; i++) {
		if(i*i == N) return 1;
	}
	return 0;
}
// O(lgN)
int func4(int N) {
/* my sol */  
//	int max = 0;
//	for(int i = 1; i*i <= N; i*=2) {
//		if(i*i > max) { // 1 2 4
//			max = i*i;
//		}
//	}
//	return max;

	int val = 1;
	while(2*val <= N) val *= 2;
	return val;
}

int main(void) {

//	cout << func1(16) << endl;
//	cout << func1(34567) << endl;
//	cout << func1(27639) << endl;
	
//	int arr[3] = {1, 52, 48};
//	cout << func2(arr, 3) << endl;
//	int arr2[2] = {50, 42};
//	cout << func2(arr2, 2) << endl;
//	int arr3[4] = {4, 13, 63, 87};
//	cout << func2(arr3, 4) << endl;
	
//	cout << func3(9) << endl;
//	cout << func3(693953651) << endl;
//	cout << func3(756580036) << endl;
	
	cout << func4(5) << endl;
	cout << func4(97615282) << endl;
	cout << func4(1024) << endl;
	
	
	return 0;
}
```


<br />
<br />

# C++, 알아두면 좋은 팁 모음
# STL과 함수 인자

```cpp
// 시간 복잡도 O(n)
bool cmp1 (vector<int> v1, vector<int> v2, int idx) {
    return v1[idx] > v2[idx];
}
// 시간 복잡도 O(1)
bool cmp2 (vector<int>& v1, vector<int>& v2, int idx) {
    return v1[idx] > v2[idx];
}
```

* cmp1에서 vector와 같은 stl은 원본을 그대로 가져오는 것이 아니라 구조체와 같이 복사하기 때문에 원본의 개수 N 만큼 복사하기 때문에 __시간복잡도가 O(N)이다.__
* cmp2에서 __참조자(&)__ 로 넘길 경우에 원본이 그대로 넘어가기 때문에 __O(1)이다.__


# 표준 입출력
* 공백을 포함한 문자열 입력받기

```cpp
// 1. scanf의 옵션: \n(줄바꿈)이 나올때까지 입력받기
char a1[10];
scanf("%[]^\n", a1);
// 2. gets 함수: 보안상 이슈로 c++14 이상에서는 제거됨
char a2[10];
gets(a2);
puts(a2);
// 3. getline 함수: 타입이 c++ string 여야한다.
string s; 
getline(cin, s);
cout << s;
```

* cin, cout 사용할때 꼭 써야하는것

```cpp
// 이걸 안해두면 입/출력 양이 많을 때 시간초과가 날 수 있다.
ios::sync_with_stdio(0), cin.tie(0)
```

보통 printf와 scanf등에서 쓰는 C stream과 cin/cout 등에서 쓰는 C++ stream은 분리가 되어있다.

printf와 cout을 번걸아하면 사용하는 상황을 생각해보자 

```cpp
cout << "11111\n";
printf("22222\n")
cout << "33333\n";
```

사용자 입장에서는 C stream와 C++ stream 분리 되어있음은 상관없이 원하는 대로 출력이 된다. 이렇게 코드의 흐름과 실제 출력이 동일하기 위해서 기본적으로 프로그램에서는 C stream와 C++ stream를 동기화 시키고 있다. 만약 내가 C++ stream 만을 사용할 것이라면 굳이 2개의 Stream을 동기화 할 필요가 없다. 쓸때 없이 시간만 잡아먹으니까... 그렇기 때문에 C++ stream 만 쓸꺼면 동기화를 끊어버려서 프로그램 수행 시간에서 이득을 챙길 수 있고 명령어는 아래이다.

* __동기화를 끊는 명령어:__ ios::sync_with_stdio(0 or false)
  + 동기화를 끊었으면 절대 cout과 printf를 섞어쓰면 안된다.
  + 섞어쓰면 출력 결과가 꼬이게 된다.


__cin.tie(0)__ 는 버퍼의 개념을 이해해야한다. cin 명령을 수행하기 전에 cout 버퍼를 비우지 않도록 하는 코드이다.

* cin.tie(nullptr)

## endl 쓰지마라!!!!!
__endl은 개행문자(\ㅜ)를 출력하고 출력 버퍼를 비워라는 명령이다.__


# References
* [바킹독의 실전 알고리즘-0x01, 기초 코드 작성 요령1](https://www.youtube.com/watch?v=9MMKsrvRiw4)
* [바킹독의 실전 알고리즘-0x02, 기초 코드 작성 요령2](https://www.youtube.com/watch?v=6lhVHP8bkPA)
* [박트리 - 복잡도 분석](https://baactree.tistory.com/26)