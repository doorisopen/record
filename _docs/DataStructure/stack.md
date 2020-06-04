---
title: 스택(Stack)
category: DataStructure
comments: true
order: 3
---

# 스택(Stack)의 성질
* __FILO(First In Last Out)__
* 출력에 제한이 있어 Restricted Structure 라고 부르기도 한다.
* 원소의 추가가 O(1)
* 원소의 제거가 O(1)
* 제일 상단의 원소 확인이 O(1)
* 제일 상단이 아닌 나머지 원소들의 확인/변경이 원칙적으로 불가능
* __선형구조__ 의 자료구조이다.


## 배열을 이용한 스택 구현

```
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
```

# 스택의 활용(0x08) - 수식의 괄호 쌍이란?

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