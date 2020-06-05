---
title:  "이진 트리(Binary Tree)"
category: DataStructure
date:   2019-07-25 18:32:30
comments: true
order: 8
---

## 이진 트리(Binary Tree) 란?
* __이진 트리는__ 가장 많이 사용되는 __비선형 자료구조__ 이다.
* __이진 트리는__ 데이터의 __탐색 속도 증진__ 을 위해 사용하는 구조이다.

## 이진 트리(Binary Tree)의 특징
* __힙 정렬(Heap Sort)을__ 구현할 때 이진 트리를 이용하여 구현할 수 있다.
* 이진 트리에서 __데이터를 탐색__ 하는 방법은 크게 __세 가지 방법이__ 있다.
  * __전위 순회 (Preorder Traversal)__
	1. __자기 자신을 먼저 처리한다.__
	2. 왼쪽 자식을 방문.
	3. 오른쪽 자식을 방문
  * __중위 순회 (Inorder Traversal)__
  	1. 왼쪽 자식을 방문.
	2. __자기 자신을 먼저 처리한다.__
	3. 오른쪽 자식을 방문
  * __후위 순회 (Postorder Traversal)__
	1. 왼쪽 자식을 방문
	2. 오른쪽 자식을 방문.
	3. __자기 자신을 먼저 처리한다.__


* __이진트리(Binary Tree)__ 형태

<a href="{{ site.datastructure_img }}/binarytreeshape.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.datastructure_img }}/binarytreeshape.JPG" title="Check out the image">
</a>

## 이진 트리(Binary Tree)의 알고리즘 예시
<a href="{{ site.datastructure_img }}/binarytree.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.datastructure_img }}/binarytree.JPG" title="Check out the image">
</a>

* __전위 순회 (Preorder Traversal)__
1 - 2 - 4 - 5 - 3 - 6 - 7
* __중위 순회 (Inorder Traversal)__
4 - 2 - 5 - 1 - 6 - 3 - 7
* __후위 순회 (Postorder Traversal)__
4 - 5 - 2 - 6 - 7 - 3 - 1


## 이진 트리(Binary Tree) C++ 구현

{% highlight javascript %}
#include <iostream>

using namespace std;

int num = 15;

// 하나의 노드 정보를 선언한다.
typedef struct node *treePointer;
typedef struct node {
	int data;
	treePointer leftChild, rightChild;
} node;

// 중위 순회
void inorder(treePointer ptr){
	if(ptr){
		inorder(ptr->leftChild);
		cout << ptr->data << ' ';
		inorder(ptr->rightChild);
	}
}
// 전위 순회 
void preorder(treePointer ptr){
	if(ptr){
		cout << ptr->data << ' ';
		preorder(ptr->leftChild);
		preorder(ptr->rightChild);
	}
}
//후위 순회 
void postorder(treePointer ptr){
	if(ptr){	
		postorder(ptr->leftChild);
		postorder(ptr->rightChild);
		cout << ptr->data << ' ';
	}
}

int main(void){
	// 데이터가 들어갈 배열을 생성 
	node nodes[num + 1];
	// 각 원소를 초기화 
	for(int i = 1; i <= num; i++){
		nodes[i].data = i;
		nodes[i].leftChild = NULL;
		nodes[i].rightChild = NULL;
	}
	// 각 노드를 연결해준다. 
	for(int i = 1; i <= num; i++){
		// 2의 배수라면 짝수라면 왼쪽에 넣어준다. 
		if(i % 2 == 0){
			nodes[i / 2].leftChild = &nodes[i];
		}
		// 홀수라면 오른쪽에 넣어준다.
		else {
			nodes[i / 2].rightChild = &nodes[i];
		}
	}
	cout<<"pre : ";
	preorder(&nodes[1]);
	cout<<endl;
	cout<<"in : ";
	inorder(&nodes[1]);
	cout<<endl;
	cout<<"post : ";
	postorder(&nodes[1]); 
	
	return 0;
}
{% endhighlight %}

### References
> * <a href="https://blog.naver.com/ndb796/221233560789">19. 이진 트리의 구현과 순회(Traversal) 방식<a>