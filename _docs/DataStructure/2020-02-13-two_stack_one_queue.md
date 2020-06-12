---
title: "스택(Stack) 2개로 큐(Queue) 구현하기"
category: DataStructure
date: 2020-02-13 00:01:30
comments: true
order: 9
---


Stack 2개를 활용하여 Queue처럼 거동하는 함수를 만들기


```
#include <iostream>
#include <stack>

using namespace std;

class myQueue {
public:
	stack<int> stackNewest, stackOldest;

	int size() {
		return stackNewest.size() + stackOldest.size();
	}
	
	void add(int value) {
		stackNewest.push(value);
	}
	
	void shiftStacks() {
		if(stackOldest.empty()) {
			while(!stackNewest.empty())	{
				stackOldest.push(stackNewest.top());
				stackNewest.pop();
			}
		}
	}
	
	void peek() {
		shiftStacks();
		cout <<  stackOldest.top() << endl;
	}
	
	void remove() {
		shiftStacks();
		int data = stackOldest.top();
		stackOldest.pop();
		cout << data << endl;
	}
}; 


int main(void) {
	myQueue q;
	q.add(1);
	q.add(2);
	q.add(3);
	q.peek();
	cout << "size: " << q.size() << endl;
	q.remove();
	q.remove();
	cout << "size: " << q.size() << endl;
	return 0;
}
```