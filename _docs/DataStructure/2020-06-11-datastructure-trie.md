---
title:  "트라이(Trie)"
category:  DataStructure
date: 2020-06-11 00:30:00
comments: true
order: 10
---


## 트라이란?
* 트라이는 __문자열의 집합을 표현하는 트리 자료구조__ 입니다.
* 트라이는 집합에 포함된 __문자열의 접두사들에 대응되는 노드들이 서로 연결된 트리__ 입니다.
* 트라이는 문자열 집합 내에서 원하는 원소를 찾는 작업을 O(M) 시간만에 할 수 있습니다.
  + M: 문자열의 최대길이

>__※참고※__<br/>
> 문자열을 다루는 작업은 정수나 실수 등의 다른 자료형을 다루는 것과 다르다. 정수나 실수형 변수는 그 크기가 항상 정해져 있기 때문에 비교에 __상수 시간__ 이 걸린다고 가정해도 되는 반면, 문자열 변수를 비교하는 데는 최악의 경우 문자열의 길이에 비례하는 시간이 걸리기 때문입니다.

## 트라이 특징
* 루트노드에서 한 노드까지 내려가는 경로에서 만나는 글자들을 모으면 해당 노드에 대응되는 접두사를 얻을 수 있기 때문에 __각 노드에 대응되는 문자열을 저장할 필요가 없다.__
* 트라이의 한 노드를 표현하는 객체는 __자손 노드들을 가리키는 포인터 목록__, __해당 노드가 종료 노드인지를 나타내는 불린 값 변수__ 로 구성된다.
  + 포인터 목록은 동적 배열로 구현하는 것이 아니라, 입력에 등장할 수 있는 모든 문자에 각각 대응되는 원소를 갖는 __고정길이 배열로 구현__ 됩니다.
  + __각 노드가 26개짜리 포인터 배열__ 을 가지고 있다.
  + 이 배열의 0번째 원소는 대응되는 문자열 맨 뒤에 글자 A를 붙이는 경우 얻을 수 있는 문자열 노드의 포인터를 나타낸다. 만약 해당 노드가 없다면 NULL을 저장한다.
* 트라이 구조는 __메모리를 엄청나게 낭비하는 단점__ 을 가지고 있지만, 다음노드를 찾는 과정에서 __어떤 탐색도 필요하지 않다는 장점__ 을 가지고 있다.

## 트라이 예시
아래 그림은 문자열 집합 arr={"BE","BET","BUS","TEA","TEN"}를 저장하는 트라이 예시입니다.

![trie]({{ site.baseurl }}{{ site.datastructure_img }}/trie.JPG)

## 트라이 구현 코드
* `toNumber()` [0, ALPABETS-1] 범위의 숫자로 변환해 주는 함수
  + 알파벳 대문자가 아니라 소문자, 숫자 등으로 출현하는 문자가 달라질 때는 이부분만 바꿔 사용한다.
* `insert()` 트라이 구조를 만드는 함수
* `find()` 해당 문자열을 찾는 함수 
  + 찾아낸 문자열에 대응되는 노드가 __종료 노드인지 아닌지에 대해서는 신경 쓰지 않는다.__
  + 트라이가 표현하는 집합에 해당 문자열이 진짜로 존재하는지 확인하기 위해서는 반환된 노드의 terminal 멤버가 참인지 확인해야 한다.
* `insert(), find()` 의 시간복잡도는 모두 __문자열의 길이 M에 선형 비례한다. O(M)__


{% highlight javascript %}
#include <iostream>
#include <string.h>
using namespace std;

const int ALPABETS = 26;
int toNumber(char ch) {
    return ch - 'A';
}
struct TrieNode {
    TrieNode* children[ALPABETS];
    bool terminal;

    TrieNode() : terminal(false) {
        memset(children, 0, sizeof(children));
    }
    ~TrieNode() {
        for (int i = 0; i < ALPABETS; i++) {
            if(children[i]) delete children[i];
        }
    }
    void insert(const char* key) {
        if(*key == 0) {
            terminal = true;
        }
        else {
            int next = toNumber(*key);
            if(children[next] == NULL) {
                children[next] = new TrieNode();
            }
            children[next] -> insert(key+1);
        }
    }
    TrieNode* find(const char* key) {
        if(*key == 0) 
            return this;
        int next = toNumber(*key);
        if(children[next] == NULL) 
            return NULL;
        return children[next] -> find(key+1);
    }
};

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    TrieNode *node = new TrieNode();

    const char* arr[5] = {"BE","BET","BUS","TEA","TEN"};
    const char* findarr[5] = {"B","BE","BU","TEAN","TEN"};
    for (int i = 0; i < 5; i++) {
        node -> insert(arr[i]);  
    }

    for (int i = 0; i < 5; i++) {
        if(node -> find(findarr[i]) != 0) {
            cout << "YES\n";
        } else {
            cout << "NO\n";
        }
    }
    return 0;
}
{% endhighlight %}


## Reference
* 종만북 - 26장 트라이