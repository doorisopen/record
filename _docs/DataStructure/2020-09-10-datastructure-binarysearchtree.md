---
title: 이진 탐색 트리(Binary Search Tree)
category: DataStructure
date: 2020-09-10 00:30:00
comments: true
order: 9
---


## 목차
* 이진 탐색 트리 란?
* 이진 탐색 트리 특징
* 이진 탐색 트리 구현
  + 삽입, 탐색, 삭제
  + 시간복잡도
* 이진 탐색 트리 활용1
  + 배열을 이진 검색 트리로 만들기


## 이진 탐색 트리(Binary Search Tree) 란?
__이진 탐색 트리__ 는 이진 트리(Binary Tree) 기반의 탐색을 위한 자료구조 입니다.

## 이진 탐색 트리 특징
이진 탐색 트리는 다음 4개의 특징을 가지고 있습니다.

* 모든 노드는 Unique 합니다. 즉 중복된 데이터가 없다는 의미입니다.
* 루트 노드를 기준으로 __왼쪽 서브 트리__ 는 루트 노드 보다 __작은 값__ 이 위치 합니다.
* 루트 노드를 기준으로 __오른쪽 서브 트리__ 는 루트 노드 보다 __큰 값__ 이 위치 합니다.
* 왼쪽, 오른쪽 서브 트리 또한 이진 탐색 트리입니다.

<a href="{{ site.baseurl }}{{ site.datastructure_img }}/datastructure-binarysearchtree.JPG" data-lightbox="falcon9-large" data-title="Check out the image">
  <img src="{{ site.baseurl }}{{ site.datastructure_img }}/datastructure-binarysearchtree.JPG" title="Check out the image">
</a>

## 이진 탐색 트리 구현
* 노드 삽입
* 노드 탐색
  + 최댓값
  + 최솟값 
* 노드 삭제
* 트리 출력(중위 순회)

이진 탐색 트리의 __기본 형태__ 는 다음과 같습니다.

```cpp
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int data) {
        this->data = data;
        this->left = nullptr;
        this->right = nullptr;
    }
};
struct BinarySearchTree {
    Node* root;
    BinarySearchTree() {
        root = nullptr;
    }
    //노드 삽입
    ...
    //노드 탐색
    ...
    //노드 삭제
    ...
    //노드 출력
    ...
};

void solve() {
    BinarySearchTree* bst = new BinarySearchTree();
    ...
}
int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    solve();
    return 0;
}
```

#### 삽입

```cpp
//노드 삽입
void insertNode(int data) {
    root = insertBST(root, data);
}
Node* insertBST(Node* root, int data) {
    if(root == nullptr) {
        root = new Node(data);
    } else if(root->data > data) {
        root->left = insertBST(root->left, data);
    } else {
        root->right = insertBST(root->right, data);
    }
    return root;
}
```

#### 탐색
```cpp
//노드 탐색
bool searchNode(int data) {
    return searchBST(root, data) ? true : false;
}
Node* searchBST(Node* root, int data) {
    if(root == nullptr) {
        return nullptr;
    } else if(root->data == data) {
        return root;
    } else if(root->data > data) {
        return searchBST(root->left, data);
    } else {
        return searchBST(root->right, data);
    }
}
Node* searchMaxNode(Node* root) {
    if(root->right == nullptr) {
        return root;
    } else {
        return searchMaxNode(root->right);
    }
}
Node* searchMinNode(Node* root) {
    if(root->left == nullptr) {
        return root;
    } else {
        return searchMinNode(root->left);
    }
}
```
#### 삭제
노드가 삭제되어도 이진 탐색 트리는 유지 되어야 하기 떄문에 고려해야할 부분이 있습니다.

* 삭제한 노드의 __자식이 없는 경우__
* 삭제할 노드의 __자식이 하나인 경우__
* 삭제한 노드의 __자식이 둘인 경우__

이때 __자식이 둘인 경우__ 에 삭제할 노드 기준으로 왼쪽 서브 트리에서 __최대값을 찾고 삭제할 노드의 부모노드와 연결__ 해야하는 점입니다. 이진 탐색 트리의 특성에서 보았듯이 이진 탐색 트리의 __오른쪽 서브 트리는 루트 노드 보다 큰 값이 오는 특징을 활용__ 한 것 입니다. 

```cpp
//노드 삭제
void deleteNode(int data) {
    if(root != nullptr) {
        deleteBST(root, data);
    } else {
        return;
    }
}
Node* deleteBST(Node* root, int data) {
    if(root->data > data) {
        deleteBST(root->left, data);
    } else if(root->data < data) {
        deleteBST(root->right, data);
    } else {
        Node* tmp = root;
        //자식이 없을때
        if(root->left == nullptr && root->left == nullptr) {
            free(root);
            root = nullptr;
        } //자식이 하나일때
        else if(root->left == nullptr) {
            root = root->right;
            delete tmp;
        } else if(root->right == nullptr) {
            root = root->left;
            delete tmp;
        //자식이 두명일때 왼쪽 노드중 가장 큰값 연결 
        } else {
            tmp = searchMaxNode(root->left);
            root->data = tmp->data;
            root->left = deleteBST(root->left, tmp->data);
        }
    }
    return root;
}
```

#### 전체 코드
```cpp
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int data) {
        this->data = data;
        this->left = nullptr;
        this->right = nullptr;
    }
};
struct BinarySearchTree {
    Node* root;
    BinarySearchTree() {
        root = nullptr;
    }
    //노드 삽입
    void insertNode(int data) {
        root = insertBST(root, data);
    }
    Node* insertBST(Node* root, int data) {
        if(root == nullptr) {
            root = new Node(data);
        } else if(root->data > data) {
            root->left = insertBST(root->left, data);
        } else {
            root->right = insertBST(root->right, data);
        }
        return root;
    }
    //노드 탐색
    bool searchNode(int data) {
        return searchBST(root, data) ? true : false;
    }
    Node* searchBST(Node* root, int data) {
        if(root == nullptr) {
            return nullptr;
        } else if(root->data == data) {
            return root;
        } else if(root->data > data) {
            return searchBST(root->left, data);
        } else {
            return searchBST(root->right, data);
        }
    }
    Node* searchMaxNode(Node* root) {
        if(root->right == nullptr) {
            return root;
        } else {
            return searchMaxNode(root->right);
        }
    }
    Node* searchMinNode(Node* root) {
        if(root->left == nullptr) {
            return root;
        } else {
            return searchMinNode(root->left);
        }
    }
    //노드 삭제
    void deleteNode(int data) {
        if(root != nullptr) {
            deleteBST(root, data);
        } else {
            return;
        }
    }
    Node* deleteBST(Node* root, int data) {
        if(root->data > data) {
            deleteBST(root->left, data);
        } else if(root->data < data) {
            deleteBST(root->right, data);
        } else {
            Node* tmp = root;
            //자식이 없을때
            if(root->left == nullptr && root->left == nullptr) {
                free(root);
                root = nullptr;
            } //자식이 하나일때
            else if(root->left == nullptr) {
                root = root->right;
                delete tmp;
            } else if(root->right == nullptr) {
                root = root->left;
                delete tmp;
            //자식이 두명일때 왼쪽 노드중 가장 큰값 연결 
            } else {
                tmp = searchMaxNode(root->left);
                root->data = tmp->data;
                root->left = deleteBST(root->left, tmp->data);
            }
        }
        return root;
    }
    //노드 출력
    void printTree(Node* root) {
        if(root == nullptr) return;
        printTree(root->left);
        cout << root->data << " ";
        printTree(root->right);
    }
};

void solve() {
    BinarySearchTree* bst = new BinarySearchTree();
    bst->insertNode(4);
    bst->insertNode(7);
    bst->insertNode(1);
    bst->insertNode(2);
    bst->insertNode(0);
    bst->insertNode(3);
    bst->printTree(bst->root);
    cout << "\n";
    bst->deleteNode(1);
    bst->printTree(bst->root);
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    solve();
    return 0;
}
```

> 이진 탐색 트리의 삽입, 삭제, 검색 시간복잡도는 <br>
> 평균: O(logN), 최악: O(N) 입니다.


## 이진 탐색 트리 활용1
* __오름차순으로 정렬된__ 배열을 이진 검색 트리로 만들기

```cpp
#include <bits/stdc++.h>
using namespace std;
struct Node {
    int data;
    Node* left;
    Node* right;
    Node(int data) {
        this->data = data;
    }
};
Node* makeTree(int arr[], int start, int end) {
    if(start > end) return NULL;
    int mid = (start + end) / 2;
    Node* node = new Node(arr[mid]);
    node->left = makeTree(arr, start, mid-1);
    node->right = makeTree(arr, mid+1, end);
    return node;
}

void searchTree(Node* root) {
    if(root == NULL) return;
    cout << root->data << " ";
    searchTree(root->left);
    searchTree(root->right);
}
void solve() {
    int arr[10] = {0,1,2,3,4,5,6,7,8,9};
    Node* root;
    root = makeTree(arr, 0, 9);
    searchTree(root);
}

int main() {
    cin.tie(nullptr);
    ios::sync_with_stdio(false);
    solve();
    return 0;
}
```

## Reference
* [엔지니어대한민국](https://www.youtube.com/watch?v=9ZZbA2iPjtM&ab_channel=%EC%97%94%EC%A7%80%EB%8B%88%EC%96%B4%EB%8C%80%ED%95%9C%EB%AF%BC%EA%B5%AD)
* [navigator-ymin.tistory.com/2](https://navigator-ymin.tistory.com/2)