---
title: 배열(Array)
category: DataStructure
date:   2020-06-04 00:30:59
comments: true
order: 1
---

## 배열
1. O(1)에 k번째 __원소를 확인/변경 가능__
2. 추가적으로 소모되는 __메모리의 양(=overhead)__ 가 거의 없음
3. __Cache hit rate__ 가 높음
4. 메모리 상에 연속한 구간을 잡아야 해서 할당에 제약이 걸림


* 마지막 원소 추가/제거: O(1)
* 임의의 위치에 원소를 확인/변경: O(1)
* 임의의 위치에 원소를 추가/제거: O(N)

```cpp
#include <bits/stdc++.h>
using namespace std;

void insert(int idx, int num, int arr[], int& len) {
    int tmp1 = arr[idx];
    arr[idx] = num;

    for(int i = idx+1; i <= len; i++) {
        int tmp2 = arr[i];
        arr[i] = tmp1;
        tmp1 = tmp2;
    }
    len++;
    // 개선
    <!-- for(int i = len; i > idx; i--) {
        arr[i] = arr[i-1];
        arr[idx] = num;
    }
    len++; -->
}
void erase(int idx, int arr[], int& len) {
    int tmp1 = arr[len-1];
    arr[len-1] = 0;
    for(int i = len-2; i >= idx; i--) {
        int tmp2 = arr[i];
        arr[i] = tmp1;
        tmp1 = tmp2;
    }
    len--;
    // 개선
    <!-- len--
    for(int i = idx; i < len; i++) {
        arr[i] = arr[i+1];
    } -->
}
void printArr(int arr[], int& len) {
    for(int i = 0; i < len; i++) {
        cout << arr[i] << " ";
    }
    cout << "\n";
}

void insert_test(){
  cout << "***** insert_test *****\n";
  int arr[10] = {10, 20, 30};
  int len = 3;
  insert(3, 40, arr, len); // 10 20 30 40
  printArr(arr, len);
  insert(1, 50, arr, len); // 10 50 20 30 40
  printArr(arr, len);
  insert(0, 15, arr, len); // 15 10 50 20 30 40
  printArr(arr, len);
}

void erase_test(){
  cout << "***** erase_test *****\n";
  int arr[10] = {10, 50, 40, 30, 70, 20};
  int len = 6;
  erase(4, arr, len); // 10 50 40 30 20
  printArr(arr, len);
  erase(1, arr, len); // 10 40 30 20
  printArr(arr, len);
  erase(3, arr, len); // 10 40 30
  printArr(arr, len);
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    
    insert_test();
    erase_test();

    return 0;
}
```

## 사용 팁(초기화)

```cpp
int a[21];
int b[21][21];

// 1. memset 비추천
memset(a, 0, sizeof a);
memset(b, 0, sizeof b);

// 2. for 무난함
for(int i = 0; i < 21; i++) {
    a[i] = 0;
}
for(int i = 0; i < 21; i++) {
    for(int j = 0; j < 21; j++) {
        b[i][j] = 0;
    }
}
// 3. fill 추천!!!!
fill(a, a+21, 0);
for(int i = 0; i < 21; i++) {
    fill(b[i], b[i]+21, 0);
}

char flags[26][80];
std::fill( &flags[0][0], &flags[0][0] + sizeof(flags) /* / sizeof(flags[0][0]) */, 0 );
```

## vector

* vector 순회

```cpp
vector<int> v1 = {1,2,3,4,5,6};

// 1. range-based for loop(since c++11)
for(int e : v1) { cout << e << ' ';}
// 2. not bad
for(int i = 0; i < v1.size(); i++) { cout << v1[i] << ' ';}
// 3. bad
for(int i = 0; i <= v1.size()-1; i++) { cout << v1[i] << ' ';}
```

* vector의 size 메소드는 unsigned int 를 반환한다. 
* unsigned int 와 int 를 연산하면 unsigned int로 자동으로 형변환이 된다.
* 만약 size가 0일때 -1 연산이 되면 4294967295가 된다. 이는 unsigned int overflow에 의한 출력 값이다.


```cpp
// 연습문제: O(n)
int func2(int arr[], int N) {
    int visit[101] = {};
    for(int i = 0; i < N; i++) {
        if(visit[100-arr[i] == 1]) {
            return 1;
        }
        visit[arr[i]] = 1;
    }
    return 0;
}
```

## References
* [바킹독의 실전 알고리즘-0x03, 배열](https://www.youtube.com/watch?v=mBeyFsHqzHg)