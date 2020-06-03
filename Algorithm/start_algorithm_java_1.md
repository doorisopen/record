## 알고리즘 시작하기 JAVA 1편
Java로 PS(Problem Solving)을 시작할때 필수로 알아야할 기본기를 정리하였습니다. 목차는 아래와 같습니다.

* 입출력(IO, Input Output)
  + System.out(Output)
  + Scanner & BufferReader(Input)
    - 다차원 배열 입력 다루기
* 배열(1차원, 다차원)
  + 선언, 생성, 이용
  + 초기화 방법(Arrays.fill)
* 문자열(String 클래스)
  + 문자열 처리(StringTokenizer, StringBuilder)


## Java의 출력(Output, System.out)

```
int num = 5;
System.out.println("java"); // 개행 포함
System.out.print("hello"); // 개행 미포함
System.out.print("world"+num); // 개행 미포함

/*Result
    java
    helloworld5
*/ 
```

## Java의 입력(Input, Scanner & BufferReader)
### Scanner 사용법

```
import java.util.Scanner;

// Scanner 선언
Scanner sc = new Scanner(System.in);

//== ex1 ==//
int a, b;
a = sc.nextInt(); // 3 입력
b = sc.nextInt(); // 5 입력
System.out.println(a + b); // 8 출력

//== ex2, 1차원 배열 입력받기 ==//
/* 
 n=5
 2 3 4 1 2 3 입력받기
*/
int n = sc.nextInt(); // 5 입력
int arr[] = new int[n];

for(int i = 0; i < n; i++) {
    arr[i] = sc.nextInt(); // 2 3 4 1 2 3 입력
}

//== ex3, 2차원 배열 입력받기1 ==//
/*
 n = 3
 1 2 3
 4 5 6
 7 8 9 입력받기
*/
int n = sc.nextInt();
int num[][] = new int[n][n];

for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        num[i][j] = sc.nextInt();
    }
}

//== ex3-1, 2차원 배열 입력받기2 ==//
/*
 n = 3
 123
 456
 789 입력받기
*/
int n = sc.nextInt();
int num[][] = new int[n][n];
String str;

for (int i = 0; i < n; i++) {
    str = sc.next(); // 공백 이전까지 읽는다.
    //nextLine() 한줄을 읽는다.
    for (int j = 0; j < n; j++) {
        num[i][j] = str.charAt(j) - 48;
    }
}

//문자 하나 입력받기
char c = sc.next().charAt(0); // hello 입력
System.out.println(c); // h 출력
```

## BufferedReader 사용법
* Scanner는 매우 편리하지만 속도가 느리기 때문에, 입력을 많이 받아야 하는 경우에는 BufferedReader를 사용하는 것이 훨씬 좋다.
* __bufferedReader에서는 read와 readLine만 있기 때문에, 정수는 파싱을 해야한다.__

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String args[]) throws IOException {
        // BufferedReader 선언
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

        //== ex1, 1차원 배열 ==//
        /*
            10 20 30 40 입력
        */
        String[] line = bf.readLine().split(" ");
        String a = line[0] + line[1];
        String b = line[2] + line[3];
        System.out.println(a + " " + b); // 1020 3040 출력
        long result = Long.valueOf(a) + Long.valueOf(b);
        System.out.println(result); // 4060 출력

        //== ex2, 2차원 배열 ==//
        /*
            n = 3
            abc
            def
            ghi 입력
        */
        int n = Integer.parseInt(bf.readLine());
        char line[][] = new char[n][n];
        
        for (int i = 0; i < n; i++) {
            String str = bf.readLine();
            for (int j = 0; j < n; j++) {
                line[i][j] = str.charAt(j);
            }
        }
        /*
            a b c
            d e f
            g h i 출력
        */
        for (int i = 0; i < line.length; i++) {
            for (int j = 0; j < line.length; j++) {
                System.out.print(line[i][j]+" ");
            }
            System.out.println();
        }
    }
}
```

## 배열 선언, 생성, 이용(1차원)
* ex1.타입 식별자[] = new 타입[크기]; 
  + int num[] = new int[5];
* ex2.타입[] 식별자 = new 타입[크기];
  + int[] num = new int[5];

```
int num[] = new int[5];
float num2[] = new float[5];
String arr[] = new String[5];

num[0] = 12;
num[1] = 10;
num[2] = num[0] + num[1];

System.out.println(num[0]); // 12
System.out.println(num[1]); // 10
System.out.println(num[2]); // 22
```

## 배열 선언, 생성, 이용(다차원)
* ex1.타입 식별자[][] = new 타입[크기: 행][크기: 열]; 
  + int num[] = new int[5];
* ex2.타입[][] 식별자 = new 타입[크기: 행][크기: 열];
  + int[] num = new int[5];

```
int num[][] = new int[5][5];
float num2[][] = new float[5][5];
String arr[][] = new String[5][5];

num[0][0] = 12;
num[1][1] = 10;
num[2][2] = num[0][0] + num[1][1];

System.out.println(num[0][0]); // 12
System.out.println(num[1][1]); // 10
System.out.println(num[2][2]); // 22
```

## 배열 초기화 방법

```
// 1차원 배열
//== ex1 ==// 
int num1[] = new int[5];
num1[0] = 10;
num1[1] = 20;
num1[2] = 30;
num1[3] = 40;
num1[4] = 50;

//== ex2 ==// 
int num2[] = {10, 20, 30, 40, 50};

//1차원 배열 크기 확인
System.out.println(num2.length); // 5


// 2차원 배열
//== ex1 ==// 
int num3[][] = {
        {1,2,3,4,5},
        {6,7,8,9,10}
    };

//2차원 배열 크기 확인
System.out.println(num3.length); // 2
System.out.println(num3[0].length); // 5
```

### Arrays.fill()

```
import java.util.Arrays;

//== ex1. 1차원 배열 초기화 ==//
int arr1[] = {1,2,3,4,5};
for (int i = 0; i < arr1.length; i++) {
    System.out.print(arr1[i]); // 1 2 3 4 5
}
//초기화
Arrays.fill(arr1, 0);
for (int i = 0; i < arr1.length; i++) {
    System.out.print(arr1[i]); // 0 0 0 0 0
}

//== ex2. 2차원 배열 초기화 ==//
int arr2[][] = {{1,2,3,4,5},{6,7,8,9,10}};
for (int i = 0; i < arr2.length; i++) {
    for (int j = 0; j < arr2[i].length; j++) {
        System.out.print(arr2[i][j]);
    }
    System.out.println();
}
//초기화
for (int i = 0; i < arr2.length; i++) {
    Arrays.fill(arr2[i], 0);
}
```

## 문자열(String 클래스)



## 문자열 처리(StringTokenizer)
### StringTokenizer
* 문자열을 토큰으로 잘라야 할 때 사용하면 편하다.

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;
​
public class Main {
  public static void main(String[] args) throws IOException {
    //BufferedReader 선언
    BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

    //== ex1: 공백 포함==//
    /*
        1 2 3 4 5 입력
    */
    String line = bf.readLine();
    StringTokenizer st = new StringTokenizer(line, " ");
    int sum = 0;
    while(st.hasMoreTokens())
      sum += Integer.valueOf(st.nextToken());
    System.out.println(sum); //15 출력

    //== ex2: 문자열 포함==//
    /*
        1,2,3,4,5 입력
    */
    String line = bf.readLine();
    StringTokenizer st = new StringTokenizer(line, ",");
    int sum = 0;
    while(st.hasMoreTokens())
      sum += Integer.valueOf(st.nextToken());
    System.out.println(sum); //15 출력
  }
}
```

### StringBuilder
* 출력해야 하는 것이 많은 경우에는, 매번 출력하는 것 보다
* StringBuilder를 이용해 문자열을 만들고, 한번에 출력하는 것이 속도면에서 좋다.


|  <center>메소드</center> |  <center>기능</center> |
|:--------:|:--------:|
|StringBuilder append(String str)|문자열 뒤에 str을 덧붙임|
|StringBuilder insert(int offset, String str)|문자열의 offset 위치에 str을 삽입|
|StringBuilder delete(int start, int end)|start부터 end-1까지의 부분 문자열을 삭제|
|StringBuilder deleteCharAt(int idx)|idx 위치에 있는 하나의 문자를 삭제|

```
import java.util.Scanner;
​
public class Main {
  public static void main(String[] args) {
    //==바로 출력하기==//
    Scanner sc = new Scanner(System.in);
    int a = sc.nextInt();
    for (int i = 1; i <= a; i++)
      System.out.print(i);

    //==한번에 출력하기==//
    StringBuilder sb = new StringBuilder(); // 기본 버퍼 크기: 16
    for (int i = 1; i <= a; i++)
      sb.append(i);
    System.out.println(sb);
    System.out.println(sb.length() + " " + sb.capacity());//5 16
  }
}
```

# References
* 뇌를 자극하는 Java 프로그래밍
* [https://developer.android.com/reference/java/util/Arrays.html](https://developer.android.com/reference/java/util/Arrays.html)
* [개발자의 기록습관](https://ict-nroo.tistory.com/61)