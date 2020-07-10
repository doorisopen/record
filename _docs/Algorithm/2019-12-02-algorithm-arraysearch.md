---
title: "정렬된 배열 & 정렬되지 않은 배열 탐색 방법"
category: Algorithm
date: 2019-12-02 01:34:59
comments: true
order: 25
---


## 정렬되지 않은 배열에서의 탐색 방법
#### 1) 순차 탐색
* 가장 간단하고 직접적인 탐색 방법
* 정렬되지 않은 배열의 항목들을 처음부터 마지막까지 하나씩 검사하여 원하는 항목을 찾아가는 방법이다
* 정렬되어 있지 않은 배열의 순차 탐색은 이해와 구현이 쉬움 그러나 배열이 많은 항목을 갖는 경우 __매우 비효율적인 방법__
* __비교횟수를 줄임으로써 순차 탐색을 개선__ 시킨다

{% highlight javascript %}
#include <stdio.h>
#define N 10

int list[N]; 

int sequence_search(int key, int low, int high){
	int i;
    list[high + 1] = key;
    for(i = low; list[i] != key; i++)
	;
	
    if(i == (high + 1)) return -1; 
    else return i;  
}

int main(void) {
	
	for(int i = 0; i < N; i++) {
		list[i] = i;
	}
	
	printf("result : %d ",sequence_search(7, 0, N-1));
	
	return 0;
}
{% endhighlight %}

* 리스트의 끝을 테스트하는 비교연산을 줄이기 위해 리스트의 끝에 찾고자 하는 키 값을 저장하고 반복문의 탈출 조건을 키 값을 찾을 때까지로 설정
* i 값이 리스트의 끝에 도달하였는지 매번 비교하지 않아도 되므로 비교연산의 수를 반으로 줄일 수 있다
* 따라서, __순차 탐색의 시간 복잡도 O(n)__ 이다


## 정렬된 배열에서의 탐색 방법
#### 1) 순차 탐색
* __순차 탐색__ 중 키 값보다 큰 항목을 만나면 배열의 마지막까지 검색하지 않고도 탐색 항목이 없을음 알 수 있다
* 항목이 배열에 뒷부분에 존재해도 원래의 __순차 탐색 방법__ 과 큰 차이가 없다
* 따라서, __시간 복잡도는 O(n)__ 으로 차이가 없다

#### 2) 이진 탐색
* 배열의 중앙에 있는 값을 조사하여 찾고자 하는 항목이 왼쪽 또는 오른쪽 부분 리스트에 있는지 알아내어 탐색의 범위를 반으로 줄인다
* __이진 탐색__ 의 구현 방법은 __재귀 호출__ & __반복 호출 방식__ 이 있다.
* 따라서, __시간 복잡도는 O(logN)__

#### 3) 색인 순차 탐색(indexed Sequential Search)
* 인덱스라 불리는 테이블을 사용하여 탐색의 효율을 높이는 방법이다

#### 4) 보간 탐색(interploation Search)
* 사전이나 전화번호부처럼 탐색 키가 존재할 위치를 예측하여 탐색하는 방법이다


## References
> * <a href="http://blog.naver.com/PostView.nhn?blogId=dlwoen9&logNo=220858549082&parentCategoryNo=&categoryNo=22&viewDate=&isShowPopularPosts=false&from=postView">[출처] [12. 탐색] 정렬되지 않은 배열에서의 탐색|작성자 훈민정음<a>
