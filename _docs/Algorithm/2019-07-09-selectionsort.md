---
title: "선택 정렬(Selection Sort)"
category: Algorithm
date: 2019-07-09 17:21:59
comments: true
order: 4
---


## 선택 정렬(Selection Sort) 이란?
선택 정렬(selection sort)은 제자리 정렬 알고리즘의 하나로, 다음과 같은 순서로 이루어진다.

1. 주어진 리스트 중에 최소값을 찾는다.
2. 그 값을 맨 앞에 위치한 값과 교체한다.
3. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.

* 비교하는 것이 상수 시간에 이루어진다는 가정 아래, n개의 주어진 리스트를 이와 같은 방법으로 정렬하는 데에는 Θ(n^2)만큼의 시간이 걸린다.

* 선택 정렬은 알고리즘이 단순하며 사용할 수 있는 메모리가 제한적인 경우에 사용 시 성능 상의 이점이 있습니다.


## 선택 정렬(Selection  Sort) 알고리즘의 특징
* 가장 작은 수를 선택해서 자리 교체를 한다.
* 비교할 개수가 많으면 비효율적인 알고리즘이다.


## 선택 정렬(Selection Sort)의 알고리즘 예시
* 배열에 1 10 5 7 3 2 9 8 6 4가 저장되어 있다고 가정하고 오름차순으로 정렬해 보자.


<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/selectionsort.jpg" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/selectionsort.jpg" title="Check out the image">
</a>


## Source Code
{% highlight javascript %}
#include <stdio.h>
int main(){
	int num[11] = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};
	int i, j, min, index, tmp;
	int r;
	for(i=0; i<10; i++){
		min = 9999;
		for(j=i; j<10; j++){
			if(min > num[j]){
				min = num[j];
				index = j;
			}
		}
		tmp = num[i];
		num[i] = num[index];
		num[index] = tmp;
	}
	for(r=0; r<10; r++){
		printf("%d ", num[r]);
	}
	return 0;
}
{% endhighlight %}


## 선택 정렬(Selection  Sort)의 시간복잡도
>
> * 시간복잡도를 계산하면...
> * 첫 for 루프 n-1회 반복 후 배열을 첫 번째 원소 정렬
> * (n-1) + (n-2) + ... + 2 + 1 = n(n-1)/2 -> 등차수열의 합
> 
> 
> * n(n-1)/2 = (n^2-n)/2 이므로 
<h4>따라서 버블 정렬의 시간 복잡도 Θ(n^2)</h4>
  * 시간복잡도 계산할 때는 <strong>상수</strong>는 무시한다.



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
> * <a href="https://gmlwjd9405.github.io/2018/05/06/algorithm-selection-sort.html">https://gmlwjd9405.github.io/2018/05/06/algorithm-selection-sort<a>
> * <a href="https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC">선택 정렬 – 위키백과<a>
