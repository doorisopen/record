---
title: "힙 정렬(Heap Sort)"
category: Algorithm
date: 2019-07-17 10:30:59
comments: true
order: 10
---

## 힙 정렬(Heap Sort) 이란?
* 힙 정렬(Heap Sort) 이란 __최대 힙 트리(내림차순 정렬)__ 나 __최소 힙 트리(오름차순 정렬)__ 를 구성해 정렬을 하는 방법이다.
* 힙 정렬은 힙 트리 구조(Heap Tree Structure)를 이용하는 정렬 방법이다.


<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/maxminheap.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/maxminheap.JPG" title="Check out the image">
</a>



## 힙 정렬(Heap Sort) 알고리즘의 특징



## 힙 정렬(Heap Sort)의 알고리즘 예시
* 전체 트리 구조를 최대 힙 구조로 바꾼다.
* Step 1. 1번 노드(부모노드)는 2번, 3번(자식노드)과 비교한다. 만약 1번 노드가 2번 3번 보다 작다면 교체하고 부모노드 보다 값이 큰 자식노드가 없다면 다음 영역(연두색 상자)으로 넘어간다.
* Step 2. 2번 노드(6) 4번 노드(8) 자식노드가 부모노드보다 값이 크기 때문에 교체한다. 
* Step 3. 1번 노드와 2번 노드 비교 후 교체한다.
* Step 4. 2번 노드와 5번 노드 비교한다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapsort1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapsort1.JPG" title="Check out the image">
</a>

* Step 5. 3번과 6번 비교후 Step 6과 같이 3번 노드의 부모노드와 비교한다.
* Step 7, 8번도 동일하게 수행한다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapsort2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapsort2.JPG" title="Check out the image">
</a>

* for루프는 노드의 개수만큼만 수행한다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapsort3.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapsort3.JPG" title="Check out the image">
</a>

* 아래의 소스의 결과로 최대 힙 구조로 변경이 된다. 
>
> __힙 구조로 변경하는데 걸리는 시간 복잡도는 O(N * logN)이다.__
> 
{% highlight javascript %}
// 전체 트리 구조를 최대 힙 구조로 바꾼다. 
	for(int i = 1; i < num; i++){
		int c = i;
		do {
			// 특정 원소의 부모 
			int root = (c - 1) / 2;
			if(heap[root] < heap[c]){
				int tmp = heap[root];
				heap[root] = heap[c];
				heap[c] = tmp;
			}
			c = root;
			for(int i=0; i < num; i++){
				printf("%d ",heap[i]);
			}
			printf("\n");
		} while (c != 0);
		
	}
{% endhighlight %}

* 다음은 정렬을 수행 하는 과정이다. 루트노드를 맨 뒤쪽으로 보내면서 트리의 크기를 1개 씩 빼준다.
* 9와 6을 바꾼 뒤 9는 정렬이 완료 됬으므로 빨간색으로 칠한다.
* Step 2 에서 힙 생성 알고리즘(Heapify)를 수행한다. 
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapify1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapify1.JPG" title="Check out the image">
</a>

* 루트의 8을 맨 뒷쪽의 원소와 교체하고 빨간색으로 칠하고 트리의 크기 1을 빼준다. 그리고 이 과정을 반복해 준다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapify2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/sort/heapify2.JPG" title="Check out the image">
</a>


## Source Code

{% highlight javascript %}
#include <stdio.h>

int num = 9;
int heap[9] = {7, 6, 5, 8, 3, 5, 9, 1, 6};

int main(void){
	// 전체 트리 구조를 최대 힙 구조로 바꾼다. 
	for(int i = 1; i < num; i++){
		int c = i;
		do {
			// 특정 원소의 부모 
			int root = (c - 1) / 2;
			if(heap[root] < heap[c]){
				int tmp = heap[root];
				heap[root] = heap[c];
				heap[c] = tmp;
			}
			c = root;
			for(int i=0; i < num; i++){
				printf("%d ",heap[i]);
			}
			printf("\n");
		} while (c != 0);
		
	}
	// 크기를 줄여가며 반복적으로 힙을 구성한다.
	for(int i = num - 1; i >= 0; i--){
		int tmp = heap[0];
		heap[0] = heap[i];
		heap[i] = tmp;
		int root = 0;
		int c = 1;
		do {
			c = 2 * root + 1;
			// 자식 중에 더 큰 값을 찾기
			if(heap[c] < heap[c+1] && c < i-1){
				c++;
			}
			// 루트보다 자식이 더 크면 교환 
			if(heap[root] < heap[c] && c < i){
				int tmp = heap[root];
				heap[root] = heap[c];
				heap[c] = tmp;
			}
			root = c;
		} while (c < i);
	} 
	printf("결과 \n");
	// 결과 출력 
	for(int i=0; i < num; i++){
		printf("%d ",heap[i]);
	}
	
	return 0;
}
{% endhighlight %}


## 힙 정렬(Heap Sort)의 시간복잡도
>
> * __힙 정렬의 시간복잡도는 O(N*logN) 이다.__
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
> * <a href="https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html">https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html<a>
> * <a href="https://ko.wikipedia.org/wiki/%ED%9E%99_%EC%A0%95%EB%A0%AC">힙 정렬 – 위키백과<a>
> * <a href="https://blog.naver.com/ndb796/221228342808">https://blog.naver.com/ndb796/221228342808<a>