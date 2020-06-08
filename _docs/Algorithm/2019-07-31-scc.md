---
title: "강한 연결 요소(Strongly Connected Component)"
category: Algorithm
date: 2019-07-31 16:35:59
comments: true
order: 19
---

## 강한 연결 요소(Strongly Connected Component)란?
* __강한 연결 요소(Strongly Connected Component)__ 란 __'강하게 연결된 정점 집합'__ 을 의미한다. 서로 긴밀하게 연결되어 있다고 하여 강한 연결 요소라고 말한다.
* 방향 그래프에서 어떤 그룹 A에 있는 임의의 두 정점 x,y에 대해서 항상 A -> B로 가는 경로가 존재한다면 그 그룹을 SCC라 칭한다.




## 강한 연결 요소(Strongly Connected Component) 알고리즘의 특징
* 같은 __강한 연결 요소(Strongly Connected Component)에__ 속하는 두 정점은 서로 도달이 가능하다.
* 사이클이 발생하는 경우 무조건 SCC에 해당한다.
* SCC는 방향(directed) 그래프일 때만 의미가 있다. 무향(Undirected) 그래프라면 그 그래프 전체는 무조건 SCC이기 때문이다.
* SCC를 추출하는 대표적인 알고리즘은 __코사라주 알고리즘(Kosaraju's Algorithm)__ 과 __타잔 알고리즘(Tarjan's Algorithm)__ 이 있다.
*  __코사라주 알고리즘(Kosaraju's Algorithm)__ 을 이용한 방법이 더 쉽게 구현이 가능하다. 그러나 __타잔 알고리즘(Tarjan's Algorithm)__ 이 적용하기 더 쉽다.
  + __타잔 알고리즘(Tarjan's Algorithm)__ 은 모든 정점에 대해 DFS를 수행하여 SCC를 찾는 알고리즘이다.


## 강한 연결 요소(Strongly Connected Component)의 알고리즘 예시
* SCC가 되기 위해서는 DFS를 수행하다가 부모로 돌아올 수 있어야 SCC가 성립 된다. 즉, 이를 검증하기 위해 부모에서 자식으로 나아가는 알고리즘으로 DFS 알고리즘이 사용된다.

* __Step 1.__ 처음에 정점 1을 기준으로 DFS를 수행한다. 다음과 같이 정점 1이 스택에 들어간다. 부모 값은 자기 자신이다.
* __Step 2.__ 이후 연결된 정점 2를 스택에 넣는다. 부모 값은 자기 자신이다.
* __Step 3.__ 정점 3도 동일하다.
* __Step 4.__ 정점 3에 대한 DFS를 수행하면 정점 1을 만나게 된다. 이 때 정점 3의 부모 값이 1임을 파악한다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/scc_1.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/scc_1.JPG" title="Check out the image">
</a>

* __Step 5.__ 정점 3에대한 DFS가 끝나고 정점 2의 부모 값 또한 정점 3의 부모값으로 갱신된다. 따라서 정점 2의 부모값은 1이다.
* __Step 6.__ 정점 2에대한 DFS가 끝나고 정점 1의 부모 값이 정점 2의 부모값과 동일 함을 확인하고 SCC임을 확인한다.
* __Step 7.__ 동일하게 정점 4부터 차례로 5 6 7 을 방문하고 스택에 넣어준다. 5 6 7 또한 각각 정점에 대해서 DFS를 수행하고 SCC를 확인한다.
* __Step 8.__ 정점 6에서 부모 값이 5임을 확인하고 5 6 7 또한 SCC가 된다. 

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/scc_2.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/scc_2.JPG" title="Check out the image">
</a>

* __Step 9.__ 정점 4또한 스택에서 빠져 나오면서 부모 값이 자기 자신임을 확인하고 SCC임을 확인한다.
* __Step 10.__ 남은 정점에 대해서도 동일하게 수행한다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/scc_3.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/scc_3.JPG" title="Check out the image">
</a>


> __강한 연결 요소(SCC)__ 는 그 자체로는 큰 의미가 없고, __위상 정렬__ 과 함께 생각해 보았을 때 큰 의미가 있다.<br>
> 모든 SCC를 각 정점으로 고려했을 때 각 SCC를 위상 정렬할 수 있다.


## Source Code

{% highlight javascript %}
#include <iostream>
#include <vector>
#include <stack>
#define MAX 10001

using namespace std;

int id, d[MAX];// 각 아이디마다 고유한 노드 저장. 
bool finished[MAX];// 특정 노드에 대한 DFS가 끝났는지 확인하는것. 
vector<int> a[MAX]; // 실질적으로 인접한 노드를 저장 
vector<vector<int> > SCC; // 한 그래프에서 여러개 나올 수 있기때문에  
stack<int> s;

// DFS는 총 정점의 갯수만큼 실행된다.
int dfs(int x){
	d[x] = ++id;
	s.push(x); // 스택에 자기 자신을 삽입한다.
	
	int parent = d[x];
	for(int i = 0; i < a[x].size(); i++) {
		int y = a[x][i];
		// 방문하지 않은 이웃 
		if(d[y] == 0) parent = min(parent, dfs(y));
		// 처리 중인 이웃 
		else if(!finished[y]) parent = min(parent, d[y]);
		
	}
	
	// 부모노드가 자기 자신인 경우
	if(parent == d[x]) {
		vector<int> scc;
		while(1){
			int t = s.top();
			s.pop();
			scc.push_back(t);
			finished[t] = true;
			if(t == x) break;
		}
		SCC.push_back(scc);
	} 
	return parent;
} 

int main(void) {
	int v = 11;
	a[1].push_back(2);
	a[2].push_back(3);
	a[3].push_back(1);
	a[4].push_back(2);
	a[4].push_back(5);
	a[5].push_back(7);
	a[6].push_back(5);
	a[7].push_back(6);
	a[8].push_back(5);
	a[8].push_back(9);
	a[9].push_back(10);
	a[10].push_back(11);
	a[11].push_back(3);
	a[11].push_back(8);
	
	for(int i = 1; i <= v; i++) {
		if(d[i] == 0) dfs(i);
	}
	printf("SCC의 개수: %d\n", SCC.size());
	for(int i = 0; i < SCC.size(); i++) {
	printf("%d 번째 SCC : ", i + 1);
		for(int j = 0; j < SCC[i].size(); j++) {
			printf("%d ", SCC[i][j]);
		}
		printf("\n");
	}
	return 0;
}
{% endhighlight %}





## 강한 연결 요소(Strongly Connected Component)의 시간복잡도
> __강한 연결 요소(Strongly Connected Component)__ 의
>> 시간 복잡도는 __O(V + E)__ 이다.
> 또한, __타잔 알고리즘의__ 시간 복잡도는 O(V + E)로 __위상 정렬__ 의 시간 복잡도와 동일하다.





## 관련된 Post





## References
> * <a href="https://www.crocus.co.kr/950">https://www.crocus.co.kr/950<a>
> * <a href="https://www.youtube.com/watch?v=H_Cg3-rv7RU&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=28">28강 - 강한 결합 요소(Strongly Connected Component)<a>
> * <a href="https://blog.naver.com/ndb796/221236952158">https://blog.naver.com/ndb796/221236952158<a>