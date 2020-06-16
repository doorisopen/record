---
title: "find()"
category: Cpp
date: 2020-04-05 12:53:30
comments: true
order: 2
---

## C++ STL find() 함수
* find 함수는 iterator 순차열 범위에서 원하는 값을 가진 iterator 반복자 __위치를 찾아서 반환__ 한다.
* 만약 해당 값을 찾지 못하면 반환하는 __iter 는 end() 를 가리킬 것__ 이다.
* find() 함수는 __순방향 반복자__ 를 요구하기 때문에 __순방향 반복자를 지원하는 컨테이너__ 라면 수행할 수 있다.


## 예시 코드(C++)

```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;


int main(){

    vector<int> v;

    for(int i = 1; i <= 10; i++) {
        v.push_back(i);
    }

    vector<int>::iterator iter;
    iter = find(v.begin(), v.end(), 7);
    cout << *iter << endl; // 7

    iter = find(v.begin(), v.end(), 100);
    if (iter == v.end()) {
        cout << "Not found!" << endl; // Not found!
    }

    return 0;
}
```