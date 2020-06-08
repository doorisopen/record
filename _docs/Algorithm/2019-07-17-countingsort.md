---
title: "계수 정렬(Counting Sort)"
category: Algorithm
date: 2019-07-17 10:30:59
comments: true
order: 9
---

## 계수 정렬(Counting Sort) 이란?
* 계수 정렬은 모든 정렬 알고리즘 중에서 가장 빠른 알고리즘의 한 종류이다. 물론 일반적인 상황에서는 잘 사용되지 않아 __'특수 정렬'__ 알고리즘이라고 할 수 있다. 그 이유는 메모리를 포기하고 많은 기억 공간을 사용하는 대신 속도를 빠르게 하였기 때문이다.


## 계수 정렬(Counting Sort) 알고리즘의 특징
* 계수 정렬은 __비교 정렬 알고리즘이__ 아닌 같은 숫자라도 정렬할 때 순서가 섞이지 않는 __안정 정렬__ 이다.
* 계수 정렬은 특정 __'범위 조건'이__ 있는 경우에 한해서 아주 빠른 알고리즘이다.

## 계수 정렬(Counting Sort)의 알고리즘 예시

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/countingsort1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/countingsort1.JPG" title="Check out the image">
</a>

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/countingsort2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/countingsort2.JPG" title="Check out the image">
</a>

결과 적으로 1 1 2 2 2 3 3 3 3 4 4 5 결과를 출력하게된다.


## Source Code

{% highlight javascript %}
#include <stdio.h>

int main(void){
	int tmp;
	int count[5];
	int array[30] = {
		1, 3, 2, 4, 3, 2, 5, 3, 1, 2,
		3, 4, 4, 3, 5, 1, 2, 3, 5, 2,
		3, 1, 2, 5, 3, 4, 1, 2, 1, 1
	};
	for(int i = 0; i < 5; i++){
		count[i] = 0;
	}
	for(int i = 0; i < 30; i++){
		count[array[i] - 1]++;
	}
	for(int i = 0; i < 5 ; i++){
		if(count[i] != 0){
			for(int j = 0; j < count[j]; j++){
				printf("%d ", i+1);
			}
		}
	}
	
	return 0;
} 
{% endhighlight %}


## 계수 정렬(Counting Sort)의 시간복잡도
>
> 계수 정렬의 시간 복잡도는 O(N + k) 이다.
> 


## 정렬 알고리즘 시간복잡도 비교

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/sorting_bigo_comp.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/sorting_bigo_comp.JPG" title="Check out the image">
</a>


## 관련된 Post
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-09-selectionsort">선택 정렬(Selection Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-bubblesort">버블 정렬(Buble Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-insertionsort">삽입 정렬(Insertion Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-mergesort">합병 정렬(Merge Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-quicksort">퀵 정렬(Quick Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-17-countingsort">계수 정렬(Counting Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-17-heapsort">힙 정렬(Heap Sort)<a>

## References
> * <a href="https://blog.naver.com/ndb796/221228361368">https://blog.naver.com/ndb796/221228361368<a>

