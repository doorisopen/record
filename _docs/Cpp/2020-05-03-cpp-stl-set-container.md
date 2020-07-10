---
title: "set container"
category: Cpp
date: 2020-05-03 17:32:30
comments: true
order: 3
---

> c++에서 set 컨테이너에 대해서 알아보겠습니다.

## set container
* 연관 컨테이너(associative container) 중 하나입니다.
* __노드 기반 컨테이너__ 이며 균형 이진트리로 구현되어있습니다.
* __key라 불리는 원소들의 집합__ 으로 이루어진 컨테이너 입니다.(원소 == key)
* __key값은 중복이 허용 되지 않습니다.(중요*****)__
* 원소가 insert 멤버 함수에 의해 삽입 되면, __원소는 자동으로 정렬 됩니다.__
* default 정렬기준은 less(오름차순 입니다.)

## set 사용법

## set 선언

```cpp
#include <set>
//set<type> 변수 명;
//ex) set<int> s;
//ex) set<pair<int, int> > s;
```

## set의 생성자, 연산자

```cpp
set<int> s; //기본 선언 방법
// set<int> s(정렬기준);
set<int> s2(s1); //s1을 s2에 복사
```

* 연산자 사용가능
  + "==", "!=", "<", ">", "<=", ">="

## set의 멤버 함수

```cpp
set<int> s;
```

* __s.begin();__
  + 맨 첫번째 원소를 가리키는 반복자를 리턴(참조)
  + iter = s.begin(); 으로 사용
* __s.end();__
  + 맨 마지막 원소+1를 가리킨다.
  + 원소의 끝부분을 알 때 사용합니다.
  + iter = s.end();
* __s.rbegin();__
* __s.rend();__
  + begin(), end()와 반대로 작동하는 멤버함수

```cpp
for(iter = s.rbegin(); iter != s.rend(); iter++) {
    cout << *iter << "\n";
}
```

* __s.clear();__
  + 모든 원소를 제거
* __s.count(k);__
  + 원소 k의 갯수를 반환
  + set에서는 무조건 0,1개 일것임 -> multiset은 키값이 중복이 가능하기때문에 multiset에서 사용됩니다.
* __s.empty();__
  + set이 비어있는지 확인
* __s.insert(k);__
  + 원소 k 삽입
  + 삽입시에 자동으로 정렬된 위치에 삽입됩니다.
  + 삽입이 성공 실패에 대한 여부는 리턴값(pair<iterator, bool>)으로 나오게 됩니다.
  + pair<iterator, bool>에서 first는 삽입 원소를 가리키는 반복자
  + second는 성공(true), 실패(false)를 나타냅니다.
* __s.insert(iter, k);__
  + iter가 가리키는 위치 부터 k를 삽입할 위치를 탐색하여 삽입합니다.
* __s.erase(iter);__
  + iter가 가리키는 원소를 제거합니다.
  + 제거한 다음 제거 한 원소 다음 원소를 가리키는 반복자를 리턴
* __s.erase(start, end);__
  + [start, end) 범위의 원소를 모두 제거합니다.
  + start, start+1 ... end-1, end 일때, __start부터 end-1까지__
* __s.find(k);__
  + 원소 k를 가리키는 반복자를 반환합니다.
  + 원소 k가 없다면 s.end()와 같은 반복자를 반환합니다.
* __s2.swap(s1);__
  + s1과 s2를 바꿔줍니다.
* __s.upper_bound(k);__
  + 원소 k가 끝나는 구간의 반복자
* __s.lower_bound(k);__
  + 원소 k가 시작하는 구간의 반복자
* __s.equal_range(k);__
  + 원소 k가 시작하는 구간과 끝나는 구간의 반복자 pair 객체를 반환
  + upper_bound(k), lower_bound(k)가 합쳐진 멤버함수
* __s.value_comp();__
* __s.key_comp();__
  + 정렬 기준 조건자를 반환
  + set 컨테이너에서는 두개의 함수 반환형이 같다.
* __s.size();__
  + set의 크기(원소 갯수) 반환
* __s.max_size();__
  + 최대 사이즈(남은 메모리 크기)를 반환


## set 사용 예시

* [10815: 숫자 카드](https://www.acmicpc.net/problem/10815)

```cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define MAX 100005

set<int> s;

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    int n, m, num;
   
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> num;
        s.insert(num);
    }

    cin >> m;
    for (int i = 0; i < m; i++) {
        cin >> num;
        if(s.find(num) != s.end()){ //원소를 찾으면
            cout << "1\n";
        } else { //원소 못찾으면
            cout << "0\n";
        }
    }
    return 0;
}
```

## References
* [https://blockdmask.tistory.com/79](https://blockdmask.tistory.com/79)
* [http://www.cplusplus.com/reference/set/set/](http://www.cplusplus.com/reference/set/set/)