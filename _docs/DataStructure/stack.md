---
title: 스택(Stack)
category: DataStructure
date:   2019-07-22 15:27:30
comments: true
order: 2
---

## 스택(Stack) 이란?
* 자료의 입력과 출력을 한 곳(방향)으로 제한한 자료구조

## 스택(Stack)의 용도 
1. 함수의 호출의 순서 제어(콜스택)
2. 문자열 역순으로 출력
3. 연산자 후위표기법
4. 인터럽트가 발생하여 복귀주소를 저장할 때
5. 주소지정방식 명령어의 자료 저장소
6. 재귀(Recursive) 프로그램의 순서를 제어
7. 컴파일러를 이용해 언어 번역

## 스택(Stack) 특징
* 스택은 __선형구조__ 이다.
* __LIFO(Last In First Out)__ 후입선출 구조이다.
* __push():__ 데이터 입력
* __pop():__ 데이터 삭제
* __top():__ 스택으로 할당된 기억공간에 제일 마지막에 삽입된 자료가 기억된 위치, 스택 포인터 라고도 한다.
* __boottom():__ 스택의 가장 밑바닥

## 스택(Stack)의 성질
* __FILO(First In Last Out)__
* 출력에 제한이 있어 Restricted Structure 라고 부르기도 한다.
* 원소의 추가가 O(1)
* 원소의 제거가 O(1)
* 제일 상단의 원소 확인이 O(1)
* 제일 상단이 아닌 나머지 원소들의 확인/변경이 원칙적으로 불가능
* __선형구조__ 의 자료구조이다.

## 스택(Stack)의 알고리즘 예시

<a href="{{ site.baseurl }}{{ site.datastructure_img }}/stack.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.datastructure_img }}/stack.JPG" title="Check out the image">
</a>


## C 언어를 이용한 Stack 구현

{% highlight javascript %}
#include <stdio.h>
#include <stdlib.h>

// 노드 생성 
typedef struct Node {
	int data;
	Node *next;
} Node;

// top 노드 
typedef struct top {
	Node *top;
} Stack;


void push(Stack *s, int data) {
	Node *node = (Node*)malloc(sizeof(Node));
	node->data = data;
	node->next = s->top;
	s->top = node;
}

int pop(Stack *s) {
	if(s->top == NULL) {
		printf("UnderFlow\n");
		return -1;
	}
	Node *node = s->top;
	int data = node->data;
	s->top = node->next;
	free(node);
	return data;
}
void show(Stack *s) {
	printf("----show----\n");
	Node *node = s->top;
	while(node != NULL) {
		printf("%d\n",node->data);
		node=node->next;
	}
}

int main(void) {
	Stack s;
	s.top = NULL;
	push(&s,3);
	push(&s,2);
	push(&s,1);
	push(&s,4);
	push(&s,5);
	push(&s,6);
	show(&s);
	pop(&s);
	show(&s);
	return 0;
}
{% endhighlight %}

## Java 언어를 이용한 Stack 구현

{% highlight javascript %}
public class mystack {
	public static class Node {
		private Object data;
		private Node next;
		
		public Node(Object data) {
			this.data = data;
			this.next = null;
		}
		
	}
	private Node top;
	
	public void push(Object data) {
		Node node = new Node(data);
		node.next = top;
		top = node;
	}
	
	public Object pop() {
		if(top == null) {
			System.out.println("UnderFlow");
		}
		Object data = top.data;
		top = top.next;
		
		return data;
	}
	public Object peak() {
		Object data = top.data;
		return data;
	}
	public void show() {
		System.out.println("----show----");
		Node node = top;
		while(node!=null) {
			System.out.println(node.data);
			node = node.next;
		}
	}
}
{% endhighlight %}

## C++ 배열을 이용한 스택 구현

{% highlight javascript %}
#include <iostream>
using namespace std;
#define ll long long

const int mx = 1000005;
int dat[mx];
int pos = 0;

void push(int x) { dat[pos++] = x; }
void pop() { pos--; }
int top() { return dat[pos-1]; }

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    push(1);
    push(2);
    push(3);

    cout << top(); // 3
    pop();
    cout << top(); // 2
    
    return 0;
}
{% endhighlight %}

## C++ STL Library를 사용한 Stack 구현

{% highlight javascript %}
#include <iostream>
#include <stack>
using namespace std;

int main(void){
	stack<int> s;
	
	s.push(8);
	s.push(5);
	s.push(2);
	s.pop();
	s.push(7);
	s.pop();
	while(!s.empty()){
		cout << s.top() << ' ';
		s.pop();
	}
	
	return 0;
}
{% endhighlight %}


## 스택의 활용(0x08) - 수식의 괄호 쌍이란?
* 32-{6-(2+4)*7} -> { ( ) }
* 5+{6-(12+4}*7} -> { ( } )
* 수식의 괄호 쌍이란, __주어진 괄호 문자열이 올바른지 판단하는 문제__ 이다.
* 문자열을 앞에서부터 읽어나갈 때, 닫는 괄호는 남아있는 괄호 중에서 가장 최근에 들어온 여는 괄호와 짝을 지어 없애버리는 명령이라고 생각해도 된다.

## 올바르지 않은 괄호 쌍1
* ({)} -> X
  + stack= ({
  + ) 일때 stack top은 { 이므로 올바르지 않는다.
* ({}
* ()}

## 결론(올바르지 않은 괄호 문제 해결법)
* 여는 괄호가 나오면 스택에 추가
* 닫는 괄호가 나왔을 경우,
  + 스택이 비어있으면 올바르지 않은 괄호 쌍
  + 스택의 top이 짝이 맞지 않은 괄호일 경우 올바르지 않은 괄호 쌍
  + 스택의 top이 짝이 맞는 괄호일 경우 pop
* 모든 과정을 끝낸 후 스택에 괄호가 남아있으면 올바르지 않은 괄호 쌍, 남아있지 않으면 올바른 괄호 쌍


# References
* [바킹독의 실전 알고리즘-0x05, 스택](https://www.youtube.com/watch?v=0DsyCXIN7Wg)
* [바킹독의 실전 알고리즘-0x08, (수식의 괄호 쌍)](https://www.youtube.com/watch?v=cdjjk-ryPKc)
* [https://blog.naver.com/ndb796/221230937978](https://blog.naver.com/ndb796/221230937978)
* [https://jeong-pro.tistory.com/97"](https://blog.naver.com/ndb796?Redirect=Log&logNo=221230937978&from=postView)
* [[자료구조] 연결리스트, 스택, 큐](https://m.blog.naver.com/PostView.nhn?blogId=c_18&logNo=10183843810&proxyReferer=https%3A%2F%2Fwww.google.com%2F)