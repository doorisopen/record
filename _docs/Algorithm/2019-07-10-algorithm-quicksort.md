---
title: "퀵 정렬(Quick Sort)"
category: Algorithm
date: 2019-07-10 11:21:59
comments: true
order: 8
---

## 퀵 정렬(Quick Sort) 이란?
* 대표적인 분할 정복 알고리즘으로 평균 속도가 O(N*logN) 이다.
* 퀵 정렬은 <strong>불안정 정렬</strong>에 속하며, 다른 원소와의 비교만으로 정렬을 수행하는 <strong>비교 정렬</strong>에 속한다.


## 퀵 정렬(Quick Sort) 알고리즘의 특징
* 특정한 값을 기준으로 큰 숫자와 작은 숫자를 서로 교환한 뒤에 배열을 반으로 나눈다.
* 퀵 정렬에는 '기준 값'이 있는데 이를 피벗(pivot) 이라고도 한다.
* 퀵 정렬은 이미 정렬이 되어 있는 경우 시간복잡도는 O(N^2)에 가깝다.
  * 그러나, 삽입 정렬의 경우 빠르게 해결 가능하다.

## 퀵 정렬(Quick Sort)의 알고리즘 예시

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/quicksort.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/quicksort.JPG" title="Check out the image">
</a>


## Source Code

{% highlight javascript %}
#include <stdio.h>

int num = 10;

int data[10] = {1,10,5,8,7,6,4,3,2,9};

// start : 부분집합의 첫번째 원소
// end : 부분집합의 마지막 원소
void quickSort(int *data, int start, int end){
	if(start >= end ){ // 원소가 한개인 경우 
		return;
	}
	int key = start; // 첫 번째 원소(pivot) 
	int i = start + 1; // 원소 출발지점 왼쪽 -> 오른쪽  
	int j = end; // 원소 출발지점 오른쪽 -> 왼쪽
	int tmp;
	while(i <= j) { // pivot 값 기준으로 찾은 큰 값과 작은 값이 엇갈린 경우까지 반복 
		while(data[i] >= data[key]) { // pivot 값 보다 큰 값을 만날때까지  
			i++;
		}
		while(data[j] <= data[key] && j > start) {// pivot 값 보다 작은 값을 만날 때까지 && pivot 값 보다 크지 않아야한다. 
			j--;
		}
		if( i > j){ // 엇갈린 경우에 키 값과 교체한다. 
			tmp = data[j];
			data[j] = data[key];
			data[key] = tmp;
		}else { // 엇갈리지 않았다면 큰 값과 작은 값을 교체한다. 
			tmp = data[j];
			data[j] = data[i];
			data[i] = tmp;
		}
	}
	quickSort(data, start, j-1);
	quickSort(data, j+1, end);
}
   
int main(){
	 quickSort( data, 0, num - 1 );
	 
	 for(int i = 0; i < num; i++){
	 	printf("%d ",data[i]);
	 }
	
	return 0;
}
{% endhighlight %}


## 퀵 정렬(Quick Sort)의 시간복잡도
>
> * 퀵 정렬의 시간복잡도는 O(N*logN) 이다.
>   * 최악의 경우에는 O(N^2) 이다.


## 정렬 알고리즘 시간복잡도 비교

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/sorting_bigo_comp.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/sorting_bigo_comp.JPG" title="Check out the image">
</a>


## 관련된 Post
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-09-algorithm-selectionsort">선택 정렬(Selection Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-algorithm-bubblesort">버블 정렬(Buble Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-algorithm-insertionsort">삽입 정렬(Insertion Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-algorithm-mergesort">합병 정렬(Merge Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-10-algorithm-quicksort">퀵 정렬(Quick Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-17-algorithm-countingsort">계수 정렬(Counting Sort)<a>
> * <a href="{{ site.baseurl }}/Algorithm/2019-07-17-algorithm-heapsort">힙 정렬(Heap Sort)<a>


## References
> * <a href="https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.htmll">https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort<a>
> * <a href="https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC">퀵 정렬 – 위키백과<a>
> * <a href="https://blog.naver.com/ndb796/221226813382">https://blog.naver.com/ndb796/221226813382<a>