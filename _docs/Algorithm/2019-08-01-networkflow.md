---
title: "네트워크 플로우(Network Flow)"
category: Algorithm
date: 2019-08-01 14:41:59
comments: true
order: 22
---

## 네트워크 플로우(Network Flow) 란?
* __네트워크 플로우(Network Flow)__ 는 특정한 지점에서 다른 지점으로 데이터가 얼마나 많이 흐르고 있는가를 측정하는 알고리즘이다.
* 네트워크 플로우(Network Flow)는 교통 체증. 네트워크 데이터 전송 등의 다양한 분야에 활용된다.




## 네트워크 플로우(Network Flow) 알고리즘의 특징
* __최대 유량(Max Flow)문제__ 를 해결하는 핵심 알고리즘이다.
  + 최대 유량 문제는 각 간선에 정해진 용량이 있을 때 A에서 B로 최대로 보낼 수 있는 유량의 크기를 구하는 문제이다.
* 최대 유량 문제는 단순하게 가능한 모든 경우의 수를 탐색하는 방법을 사용한다.
  + 일반적으로 __BFS(너비 우선 탐색)__ 을 이용하는 것이 일반적이다.
    - 이를 __에드몬드 카프(Edmonds-Karp) 알고리즘__ 이라고 한다.





## 네트워크 플로우(Network Flow)의 알고리즘 예시
* 가능한 모든 경우의 수를 탐색하기 위해 기본적으로 아래와 같이 현재 흐르고 있는 __유량(Flow)__ 을 모두 0으로 설정한다.
* 이후 정해진 __용량(Capacity)__ 안에서 가능한 용량의 양을 반복적으로 더해주면 된다.

* 가능한 모든 경우의 수를 탐색하기 위해 기본적으로 아래와 같이 현재 흐르고 있는 유량(Flow)을 0으로 초기화 한다. 이후 정해진 용량(Capacity) 안에서 가능한 용량의 양을 반복적으로 더해주면 된다.
<a href="{{ site.baseurl }}{{ site.algorithm_img }}/networkflow.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/networkflow.JPG" title="Check out the image">
</a>

* __Step 1.__ 1 -> 2 -> 3 -> 6 경로에서 흐를 수 있는 최소 유량을 6이므로 6만큼 흘려보내준다.
* __Step 2.__ 1 -> 2 -> 6 경로로 6만큼 흘려보내준다.
* __Step 3.__ 1 -> 2로 가는 유량이 가득차서 더이상 흘려보낼 수 없으므로 1 -> 4 -> 5 -> 6 경로로 최소 유량인 4만큼 흘려보내준다.
* __Step 4.__ 1 -> 4 -> 5 -> 3 -> 6 경로로 보낼수있는 최소 유량인 2만큼 흘려보내준다.
* __Step 5.__ 
> 이제 더이상 보낼 수 있는 방법이 없는 것처럼 보이지만 이제 여기에서 __최대 유량 알고리즘__ 의 핵심적인 부분이 등장한다. 바로 __음의 유량을 계산__ 하는 것이다. __최대 유량 알고리즘__ 은 모든 가능한 경로를 다 계산해주기 위해서 음의 유량도 계산해준다.
>> 음의 유량이란, Step 5 그래프의 예시로 현재 2 -> 3번으로 흐르는 유량이 6만큼 흐르고 있다. 그 의미는 3 -> 2 로 가는 유량은 -6 만큼 흐르고 있다고 할 수 있다.
> 이처럼 음의 유량을 기억하는 이유는 __남아있는 모든 가능한 경로를 더 찾아내기 위해서__ 이다.
* __Step 6.__ 음의 유량을 이용하여 1 -> 4 -> 5 -> 3 -> 2 -> 6 경로로 최소로 흐를수 있는 유량인 1만큼 흘려보내준다.

<a href="{{ site.baseurl }}{{ site.algorithm_img }}/networkflow_step.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.algorithm_img }}/networkflow_step.JPG" title="Check out the image">
</a>

최종적으로 __최대 유량은 19__ 만큼 흐르게 되는것을 알 수 있다.

* 추가로, 남아있는 양이 1이 넘으면 계속해서 흘려 보내주면 알아서 최적화가 이루어진다는 점에서 특별한 상황이 아니면 유량을 보내는 순서를 고려할 필요가 없다.


## Source Code

{% highlight javascript %}
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

#define MAX 100
#define INF 1000000000

using namespace std;

int n = 6, result;
// c 용량 ,f 유량 ,d 특정 노드를 방문했는지 
int c[MAX][MAX], f[MAX][MAX], d[MAX];
// 특정 노드에 연결된 간선 
vector<int> a[MAX];
// 최대 유량을 구하는 함수 
void maxFlow(int start, int end) {
	while(1) {
		
		fill(d, d + MAX, -1); // 모든 정점은 방문하지 않은 상태로 초기화 
		queue<int> q;
		q.push(start);
		while(!q.empty()) {
			int x = q.front();
			q.pop();
			for(int i = 0; i < a[x].size(); i++) {
				int y = a[x][i];
				// 방문하지 않은 노드 중에서 용량이 남은 경우
				if(c[x][y] - f[x][y] > 0 && d[y] == -1) {
					q.push(y);
					d[y] = x; // 경로를 기억합니다.
					if(y == end) break; // 도착지에 도달을 한 경우 
				} 
			}
		}
		// 모든 경로를 다 찾은 뒤에 탈출한다. 
		if(d[end] == -1) break;
		int flow = INF; // 최소값을 찾으려고
		// 거꾸로 최소 유량을 탐색한다. 
		for(int i = end; i != start; i = d[i]){
			flow = min(flow, c[d[i]][i] - f[d[i]][i]);
		} 
		// 최소 유량 만큼 추가한다.
		for(int i = end; i != start; i = d[i]){
			f[d[i]][i] += flow;
			f[i][d[i]] -= flow;
			
		}
		result += flow; 
	}
}

int main (void) {
	a[1].push_back(2);
	a[2].push_back(1);
	c[1][2] = 12;
	
	a[1].push_back(4);
	a[4].push_back(1);
	c[1][4] = 11;
	
	a[2].push_back(3);
	a[3].push_back(2);
	c[2][3] = 6;
	
	a[2].push_back(4);
	a[4].push_back(2);
	c[2][4] = 3;
	
	a[2].push_back(5);
	a[5].push_back(2);
	c[2][5] = 5;
	
	a[2].push_back(6);
	a[6].push_back(2);
	c[2][6] = 9;
	
	a[3].push_back(6);
	a[6].push_back(3);
	c[3][6] = 8;

	a[4].push_back(5);
	a[5].push_back(4);
	c[4][5] = 9;

	a[5].push_back(3);
	a[3].push_back(5);
	c[5][3] = 3;
	
	a[5].push_back(6);
	a[6].push_back(5);
	c[5][6] = 4;
	
	maxFlow(1, 6);
	printf("%d", result);
}
{% endhighlight %}


>> 실제로 네트워크 플로우 알고리즘의 경우에 에드몬드 카프 알고리즘보다 빠른 알고리즘이 존재한다.<br/>
>> 추후 __이분 매칭 알고리즘__ 등 다양한 알고리즘으로 확장 될 수 있으므로 중요한 알고리즘이다.




## 네트워크 플로우(Network Flow)의 시간복잡도
>> BFS를 이용하는 __에드몬드 카프__ 알고리즘의 시간 복잡도는 __O(V * E^2)__ 이다.







## 관련된 Post





## References
> * <a href="https://www.zerocho.com/category/Algorithm/post/5893405b588acb00186d39e0">https://www.zerocho.com/category/Algorithm/post/5893405b588acb00186d39e0<a>
> * <a href="https://www.youtube.com/watch?v=Wn51_ypG_T8&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=29">29강 - 네트워크 플로우(Network Flow)<a>
> * <a href="https://blog.naver.com/ndb796/221237111220">https://blog.naver.com/ndb796/221237111220<a>