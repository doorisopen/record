---
title: "버블 정렬(Bubble Sort)"
category: Algorithm
date: 2019-07-10 11:21:59
comments: true
order: 5
---

## 버블 정렬(Bubble Sort) 개념 요약
* 버블 정렬(Bubble sort)은 두 인접한 원소를 검사하여 정렬하는 방법이다.
* 시간 복잡도가 O(n^2)로 상당히 느리다.
* 코드가 단순하기 때문에 자주 사용된다.

## 버블 정렬(Bubble  Sort) vs 선택 정렬(Selection Sort)
* <strong>버블 정렬</strong>은 <strong>선택 정렬</strong>보다 비효율적이다.
* <strong>버블 정렬</strong>은 바로 옆의 수와 자리를 바꾸는 연산을 한다. 그러나 <strong>선택 정렬</strong>은 가장 작은 수와 자리 교체를 하 기 때문에 <strong>선택 정렬</strong>이 <strong>버블 정렬</strong> 보다 빠르다고 할 수 있다.


## 버블 정렬(Bubble Sort)의 알고리즘 예시

 * 배열에 1 4 5 2 3 이 저장되어 있다고 가정하고 오름차순으로 정렬해 보자.


<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/bubblesort.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/bubblesort.JPG" title="Check out the image">
</a>

## Source Code
{% highlight javascript %}
#include <stdio.h>

int main(){
	int num[10] = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};
	int i, j, index, tmp;
	int r;
	for(i = 0 ; i < 10; i++){
		printf("%d 번째 회전 \n",i);
		for(j = 0; j < 10 - i; j++){
			if(num[j] > num[j+1]){
				tmp = num[j+1];
				num[j+1] = num[j];
				num[j] = tmp;
				for(r = 0; r < 10; r++){
					printf("%d ",num[r]);
				}
				printf("\n");
			}
		}
	}

	return 0;
}
{% endhighlight %}


## 버블 정렬(Bubble  Sort)의 시간복잡도
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
> * <a href="https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort.html">https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort<a>
> * <a href="https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC">버블 정렬 – 위키백과<a>