---
title: "합집합 찾기(Union-Find)"
category: Algorithm
date: 2019-07-24 18:32:30
comments: true
order: 14
---

## 합집합 찾기(Union-Find) 이란?
* __합집합 찾기(Union-Find)는__ 대표적인 __그래프 알고리즘__ 이다. __서로소 집합(Disjoint-Set) 알고리즘__ 이라고도 부른다.
* __합집합 찾기(Union-Find)는__ __Disjoint Set을__ 표현할 때 사용하는 알고리즘이다.
  *  __Disjoint Set을__ 구현하는 데 __트리, 비트 벡터, 배열, 연결 리스트를__ 이용할 수 있다. 
* __합집합 찾기(Union-Find)는__ 여러 개의 노드가 존재할 때 두 개의 노드를 선택해서, 현재 이 두 노드가 __서로 같은 그래프에 속하는지 판별__ 하는 알고리즘이다.


<hr/>


## 합집합 찾기(Union-Find)의 특징
* __집합__ 을 구현하는데에 있어서 __비트 벡터, 배열, 연결 리스트__ 보다 __트리를__ 이용한 방법이 가장 __효율적이다.__
* __합집합 찾기(Union-Find)의 연산__
  * __Find() :__ 어떤 원소가 주어졌을 때 이 원소가 속한 집합을 반환.
  * __Union() :__ 두 개의 집합을 하나의 집합으로 합친다.


<hr/>


## 트리를 이용한 방법으로 구현하는 이유
>> __배열을__ 이용하여 __Union-Find를__ 구현 할 수 있지만, __Union__ 연산 수행 시 배열의 모든 원소를 순회해야 하기 때문에 __시간복잡도가 O(N)__ 이 된다. 
* __트리를__ 이용하여 __Union-Find를__ 구현하면 배열보다 빠르게 __Union__ 연산을 수행할 수 있다.


<hr/>


## 합집합 찾기(Union-Find)의 알고리즘 예시

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind1.JPG" title="Check out the image">
</a>

* __Step 1과__ 같이 여러개의 노드가 연결되지 않고 존재한다고 하자. 그리고 배열에 모든 값이 각자 자기 자신을 가리키도록 만든다. 배열의 __첫 번째 행은 '노드 번호'를__ 의미하고 __두 번째 행은 '부모 노드 번호'를__ 의미한다. 즉, _자기 자신이 어떠한 부모에 포함되어 있는지를 의미한다._ 

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind2.JPG" title="Check out the image">
</a>

* __Step 2와__ 같이 1과 2가 연결되었을때 2번 노드의 두 번째 행인 부모 노드 번호가 '1' 이 들어간다. 이렇게 부모를 합칠 때는 일반적으로 더 작은 값 쪽으로 합친다. 이것을 __Union__ 이라고 한다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind3.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind3.JPG" title="Check out the image">
</a>

* __Step 3에서__ 2와 3이 연결이 된다면 3번의 부모 노드 번호는 2가 된다. 그러나 여기서 중요한 점은 __'1과 3이 연결되었는지는 어떻게 파악할 수 있는가'__ 이다. 1과 3의 부모가 각각 1과 2로 다르기 때문에 '부모 노드'만 보고는 한번에 파악할 수 없다. 그렇기 때문에 __재귀 함수가__ 사용된다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind4.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/unionfind4.JPG" title="Check out the image">
</a>

* __Step 4와__ 같이 3의 부모를 찾기 위해서는 먼저 3이 가리키고 있는 2를 찾는다. 그러면 2의 부모가 1을 가리키고 있으므로 결과적으로 3의 부모는 1이 되는것을 파악할 수 있는 것이다.
* 이러한 과정은 __재귀적으로__ 수행될 때 가장 효과적이고 직관적으로 작성할 수 있다. 


## C++ STL Library를 사용한 합집합 찾기(Union-Find) 구현

{% highlight javascript %}
#include <stdio.h>

// 부모 노드를 찾는 함수 
int getParent(int parent[], int x) {
	if(parent[x] == x) return x;
	return parent[x] = getParent(parent, parent[x]);
} 
// 두 부모 노드를 합치는 함수 
int unionParent(int parent[],int a, int b){
	a = getParent(parent, a);
	b = getParent(parent, b);
	if(a < b) parent[b] = a;
	else parent[a] = b;
	
}

// 같은 부모를 가지는지 확인 == 같은 그래프인지 확인 
int findParent(int parent[], int a, int b){
	a = getParent(parent, a);
	b = getParent(parent, b);
	if(a == b) return 1;
	return 0;
}

int main(void){
	int parent[11];
	for(int i = 0; i <= 10; i++){
		parent[i] = i;
	}
	unionParent(parent, 1, 2);
	unionParent(parent, 2, 3);
	unionParent(parent, 3, 4);
	unionParent(parent, 5, 6);
	unionParent(parent, 6, 7);
	unionParent(parent, 7, 8);
	
	printf("1과 5가 연결되어있나? %d \n", findParent(parent, 1, 5));
	unionParent(parent, 1, 5);
	printf("1과 5가 연결되어있나? %d \n", findParent(parent, 1, 5));
	
	return 0;
}
{% endhighlight %}

## Union-Find 최악의 경우 & 최적화한 방법

#### 최악의 상황
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/unionfindworstcase.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/unionfindworstcase.JPG" title="Check out the image">
</a>

* __트리 구조가 완전 비대칭인 경우__
* 연결 리스트 형태인 경우
* 트리의 높이가 최대가 되는 경우
>> 원소의 개수가 N일 때, 트리의 높이가 N-1이므로 union과 find(x)의 시간 복잡도가 모두 O(N)이 된다.
>> 배열로 구현하는 것보다 비효율적이다.

#### find 연산 최적화
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/unionfindpathcompressionnew.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/unionfindpathcompressionnew.JPG" title="Check out the image">
</a>


## References
> * <a href="https://www.youtube.com/watch?v=AMByrd53PHM&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=18">18강 - 합집합 찾기(Union-Find)<a>
> * <a href="https://bowbowbow.tistory.com/26">https://bowbowbow.tistory.com/26<a>
> * <a href="https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html">[알고리즘] Union-Find 알고리즘<a>