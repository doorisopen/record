---
title: 재귀와 꼬리재귀
category: Algorithm
comments: true
order: 36
---

## 재귀란?
* 자기 자신(함수)을 호출하는 것을 재귀라고 한다. 

```
//재귀
public static int recursive(int n) {
    if(n == 1) return 1;
    return n * recursive(n-1);
}
```

## 꼬리재귀란?
* 재귀의 경우 스택을 사용하여 stack overflow가 발생할 수 있는데 이를 최적화 한것이 꼬리 재귀이다.


## 재귀와 꼬리재귀

```
public class Factorial {
    //재귀
    public static int recursive(int n) {
        if(n == 1) return 1;

        return n * recursive(n-1);
    }
    
    //꼬리재귀
    public static int tailRecursive(int n, int acc) {
        if(n == 1) return acc;

        return tailRecursive(n-1, n*acc);   
    }
}
```

## 재귀와 꼬리 재귀 차이점
* 재귀는 스택을 사용한다.
* 꼬리 재귀는 스택을 사용하지 않는다.

![algorithm-recursive_tailrecursive_1]({{ site.baseurl }}/images/Algorithm/algorithm-recursive_tailrecursive_1.JPG)