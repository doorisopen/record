---
title: "순차 탐색과 이분 탐색"
category: Algorithm
date: 2020-08-12 01:34:59
comments: true
order: 29
---

## 순차 탐색(Linear Search)
4,3,2,1,6,5,7,8,9 데이터가 주어질때, 7은 몇 번째 위치에 존재하는지를 찾는다고 해보겠습니다.

__순차 탐색(Linear Search)__ 은 데이터의 정렬 유무와 상관 없이 앞에서부터(4 부터) 데이터를 하나씩 확인하여 탐색하는 알고리즘 입니다. 

```cpp
#include <iostream>
using namespace std;

int main(void) {
    int data[10] = {4,3,2,1,6,5,7,0,8,9};
    int target = 7;
    for(int i = 0; i < 10; i++) {
        if(data[i] == target) {
            cout << target << " is Located in arr[" << i << "]\n";
            // Result: 7 is Located in arr[6]
        }
    }
    return 0;
}
```

## 이분(=이진) 탐색(Binary Search)
1,2,3,4,5,6,7,8,9 10개의 데이터 중에서 7을 찾을때 순차 탐색과 같이 앞쪽부터 차례로 탐색을한다면 7개의 데이터를 확인하게 됩니다. 그러나 뒷쪽부터 시작한다면 3개의 데이터만 확인하면 발견할 수 있어 빠르게 탐색이 가능하죠. 하지만 뒤쪽부터 탐색을 시작한다면 2를 탐색한다고 할때 8개의 데이터를 탐색하게 되기때문에 오히려 느려집니다.

이처럼 데이터가 정렬이 된 상황에서 데이터를 빠르게 탐색하기 위한 방법 중 하나가 __이분(=이진) 탐색(Binary Search)__ 입니다.

__이분(=이진) 탐색(Binary Search)__ 은 __이미 데이터가 정렬이 되어있는 상황__ 에서 범위를 절반씩 좁혀가며 데이터를 빠르게 탐색하는 알고리즘 입니다.

> __이분 탐색__ 은 한 번 탐색할 때마다 크기를 반으로 줄여나간다는 점에서 __O(logN)의 시간 복잡도__ 를 가집니다.

```cpp
#include <iostream>
using namespace std;
#define N 10
int data[N] = {0,1,2,3,4,5,6,7,8,9};

int binary_search(int start, int end, int target) {
    if(start > end) return -1;
    int mid = (start + end) / 2;

    if(data[mid] == target) {
        return mid;
    } else if(data[mid] > target) {
        return binary_search(start, mid-1, target);
    } else {
        return binary_search(mid+1, end, target);
    }    
}

int main(void) {
    int target = 7;
    cout << target << " is Located in arr[" << binary_search(0,N-1,target) << "]\n";
    return 0;
}
```

#### 이분 탐색 요약
* 데이터가 정렬된 상황에서 사용한다.
* 한 번 탐색할 때마다 크기를 반으로 줄여나가는 방식.
* 탐색 속도가 매우 빠르다.(시간복잡도: O(logN))
* AVL 트리, B트리, 분할 정복 등에서 넓게 적용된다.

#### std::binary_search()
이분 탐색을 구현한 c++ 라이브러리(include header: algorithm)

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
#define N 10
int data[N] = {0,1,2,3,4,5,6,7,8,9};

int main(void) {
    int target = 7;
    if(binary_search(data,data+N,target)) {
        cout << "Find target!\n";
    } else {
        cout << "Not Found!\n";
    }
    return 0;
}
```

#### std::lower_bound(), std::upper_bound()
다음 함수들은 기본적으로 __이진 탐색__ 을 사용 하는데, 주어진 값을 기준으로 하여 찾아냅니다.(때문에 데이터가 정렬되어 있어야 사용할 수 있습니다.)

* __lower_bound():__ target 값보다 크거나 __같으면서__ 가장 작은 값을 찾습니다. 없으면 구간의 끝 주소나 이터레이터를 반환합니다.
* __upper_bound():__ target 값보다 크면서 가장 작은 값을 찾습니다. 없으면 구간의 끝 주소나 이터레이터를 반환합니다.

두 함수의 차이는, __같은 값을 포함하느냐 아니냐__ 입니다.

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
#define N 10
int data[N] = {0,1,2,3,4,5,6,7,8,9};

int main(void) {
    int target = 7;
    int failTarget = 100;
    int *p;
    cout << *lower_bound(data,data+N,target) << "\n";// 7->크거나 같은 가장 작은값
    cout << *upper_bound(data,data+N,target) << "\n";// 8->크고 가장 작은값
    
    cout << "====target Index====\n";
    p = lower_bound(data,data+N,target);
    if(p != data+N) {
        cout << "target is in data[" << p-data << "]=" << *p << "\n";
    } else {
        cout << "Not Found\n";
    }
    p = upper_bound(data,data+N,target);
    if(p != data+N) {
        cout << "target is in data[" << p-data << "]=" << *p << "\n";
    } else {
        cout << "Not Found\n";
    }
    
    cout << "====Not Found====\n";
    cout << *lower_bound(data,data+N,failTarget) << "\n";// 0
    cout << *upper_bound(data,data+N,failTarget) << "\n";// 0
    return 0;
}
```


## References
* [ndb796](http://blog.naver.com/PostView.nhn?blogId=ndb796&logNo=221275413971)
* [bestmaker0290](https://m.blog.naver.com/PostView.nhn?blogId=bestmaker0290&logNo=220820005454&proxyReferer=https:%2F%2Fwww.google.com%2F) 
* [kks227](https://m.blog.naver.com/PostView.nhn?blogId=kks227&logNo=220403975420&proxyReferer=https:%2F%2Fwww.google.com%2Fhttps://m.blog.naver.com/PostView.nhn?blogId=kks227&logNo=220403975420&proxyReferer=https:%2F%2Fwww.google.com%2F)
