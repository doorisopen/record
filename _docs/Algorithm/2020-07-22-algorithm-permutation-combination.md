---
title: "순열과 조합"
category: Algorithm
date: 2020-07-22 01:34:59
comments: true
order: 27
---

__경우의 수__ 를 세는 방법 가운데 바탕이 되는 것은 __순열과 조합__ 입니다. 이번 포스팅에서는 순열과 조합에 대해서 알아보겠습니다.

## 목차
* 순열
* 중복 순열
* 조합
* 중복 조합
* 응용1 : 부분 문자열 조합


## 순열(Permutation)
1,2,3,4,5 과 같이 일련의 숫자가 주어질때, 만들 수 있는 다섯자리 자연수는 몇 개일까? 5!(5x4x3x2x1=120가지)이다. 이 가운데 3개를 뽑아 만들 수 있는 자연수는 몇 개일까? 5x4x3=60가지 입니다. 

다시 정리하면 아래와 같습니다.

* 다섯(n=5)자리 자연수 만들기 : 5!
* 5개의 일련의 숫자 중 3개 뽑기 : 5P3 (== 5!/(5-3)! == 5!/2!)
* n개의 원소 중 r개 뽑기 : n!/(n-r)!

다음은 __5개 원소 중 3개를 뽑은 결과__ 입니다.

```cpp
1 2 3
1 2 4
1 2 5
1 3 2
1 3 4
1 3 5
...
2 1 3
2 1 4
2 1 5
2 3 1
2 3 4
...
```

> 순열은 순서가 존재합니다

__순서가 존재한다는 말__ 은 위의 결과를 볼때 다음과 같은 형태를 볼 수 있다.
* 시작점 1인 경우 : [__1__, 2, 3]
* 시작점 2인 경우 : [__2__, 1, 3]

즉, [1, 2, 3, 4, 5] 5개 원소 중 어떤 원소가 시작점이던지 순서(앞에서 차례로 선택)가 존재함을 알 수 있습니다. 

다음은 __순열__ 을 구현한 코드(c++)입니다.

```cpp
// 순열 (중복을 허용하지 않습니다.)
#include <bits/stdc++.h>
using namespace std;

vector<int> arr;
bool visit[10];
int n = 5, m = 3; // n: 원소 개수, m: 순열 개수

void permutation(int d) {
    if(d == m) {
        for (int i = 0; i < arr.size(); i++) {
            cout << arr[i] << " ";
        }
        cout << "\n";
    }
    for (int i = 0; i < n; i++) {
        if(!visit[i]) {
            arr.push_back(i);
            visit[i] = true;
            permutation(d+1);
            arr.pop_back();          
            visit[i] = false;
        }
    }
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    permutation(0);
    return 0;
}
```


## 중복 순열(Duplicate Permutations)

```cpp
1 1 1 
1 1 2
1 1 3
1 1 4
1 1 5
1 2 1
1 2 2
1 2 3
1 2 4
1 2 5
...
5 5 3
5 5 4
5 5 5
```

다음은 __중복 순열__ 을 구현한 코드(c++)입니다.

```cpp
// 중복 순열 (중복을 허용합니다.)
#include <bits/stdc++.h>
using namespace std;
#define MAX 10000001

int arr[MAX];
int n = 4, m = 2; // n: 원소 개수, m: 순열 개수

void duplicate_permutation(int d) {
    if(d == m) {
        for (int i = 0; i < m; i++) {
            cout << arr[i] << " ";
        }
        cout << "\n";
        return;
    }
    for (int i = 1; i <= n; i++) {
        arr[d] = i;
        duplicate_permutation(d+1);
    }
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    
    duplicate_permutation(0);

    return 0;
}
```

## 조합(Combination)
* __순서가 사라진 순열__ 을 조합이라고 합니다.
* 5개의 일련의 숫자 중 3개 뽑는 방법? 
  + __순서가 존재하는 순열__ 의 경우 5P3 
  + __순서를 존재하지 않는 순열(조합)__ 은 5C3(== 5P3/r!) 입니다.
  + __nCr__ == nPr / r! == n! / r!(n-r)!

```cpp
1 2 3 
1 2 4 
1 2 5 
1 3 4 
1 3 5 
1 4 5 
2 3 4 
2 3 5
2 4 5
3 4 5
```

> 조합은 순서가 존재하지 않습니다

__순서가 존재하지 않는다는 말__ 은 위의 결과를 볼때 다음과 같은 형태를 볼 수 있다.
* 시작점 1인 경우 : [__1__, 2, 3]
* 시작점 2인 경우 : [__2__, 3, 4]

즉, [1, 2, 3, 4, 5] 5개 원소 중 원소 1이 시작점이 된 이후 원소 2가 시작점이 될때 이 전의 선택된 원소(원소 1)는 선택하지 않습니다. 더 자세히 설명하자면

```cpp
//시작점 1일때, 2일때
 -1-     -2-
1 2 3 | 2 1 3 ==
1 2 4 | 2 1 4 ==
1 2 5 | 2 1 5 ==
1 3 4 | 2 3 4 !=
1 3 5 | 2 3 5 !=
1 4 5 | 2 4 5 !=
```

위의 결과는 시작점이 1과 2일때 조합의 결과입니다. 가장 위에서부터 3개의 결과의 원소를 자세히 살펴보면 동일한 원소를 가지고 있음을 볼 수 있습니다. __조합은 순서와 상관없기 때문에__ 시작점이 1일때 이미 조합된 결과 이기때문에 순열과 다르게 차례로 원소를 선택하는 것이 아니라 __이전에 선택된 원소는 제외__ 합니다. 

다음은 __조합__ 을 구현한 코드(c++)입니다.

```cpp
// 조합 (중복을 허용하지 않습니다.)
#include <bits/stdc++.h>
using namespace std;

vector<int> arr;
bool visit[10];
int n = 4, m = 3; // n: 원소 개수, m: 조합할 개수

void combination(int d, int a) {
    if(d == m) {
        for (int i = 0; i < arr.size(); i++) {
            cout << arr[i] << " ";
        }
        cout << "\n";
    }
    for (int i = a; i <= n; i++) {
        if(!visit[i]) {
            arr.push_back(i);
            visit[i] = true;
            combination(d+1, i);
            arr.pop_back();          
            visit[i] = false;
        }
    }
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    
    combination(0,1);

    return 0;
}
```


## 중복 조합(Duplicate Combinations)

```cpp
1 1 1 
1 1 2 
1 1 3 
1 1 4 
1 1 5 
1 2 2 
1 2 3 
...
3 5 5
4 4 4
4 4 5
4 5 5
5 5 5
```

다음은 __중복 조합__ 을 구현한 코드(c++)입니다.

```cpp
// 중복 조합 (중복을 허용합니다.)
#include <bits/stdc++.h>
using namespace std;
#define MAX 10000001

int arr[MAX];
int n = 4, m = 3; // n: 원소 개수, m: 조합할 개수

void duplicate_combination(int d, int a) {
    if(d == m) {
        for (int i = 0; i < m; i++) {
            cout << arr[i] << " ";
        }
        cout << "\n";
        return;
    }
    for (int i = a; i <= n; i++) {
        arr[d] = i;
        duplicate_combination(d+1, i);
    }
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    
    duplicate_combination(0, 1);
    return 0;
}
```

## 응용1: 부분 문자열 조합
길이가 N인 문자열이 주어질때 __중복없이 가능한 모든 부분 문자열의 조합__ 을 구하시오.

```javascript
// str = "ABCD"
// Result
[ 'A' 'B' 'C' 'D' ]
[ 'A' 'B' 'CD' ]
[ 'A' 'BC' 'D' ]
[ 'A' 'BCD' ]
[ 'AB' 'C' 'D' ]
[ 'AB' 'CD' ]
[ 'ABC' 'D' ]
[ 'ABCD' ]
```

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

vector<string> out;

void printAll() {
    cout << "[ ";
    for (auto str : out) {
        cout << str << " ";
    }
    cout << "]\n";
}
void solution(string S) {
    if(S.length() == 0) {
        printAll();
        return;
    }

    for (int i = 0; i < S.length(); i++) {
        out.push_back("'"+S.substr(0,i+1)+"'");
        solution(S.substr(i+1));
        out.pop_back();
    }
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    string test1 = "ABCD";

    solution(test1);

    return 0;
}
```

## References
* [techiedelight](https://www.techiedelight.com/find-combinations-non-overlapping-substrings-string/)
* [yabmoons](https://yabmoons.tistory.com/99)
* [suhak](https://suhak.tistory.com/2)