# story7. 클래스 정보, 어떻게 알아낼 수 있나?
생성일: 2022년 6월 17일 오후 10:37

---

# Reflection 관련 클래스

자바 API에는 **reflection** 이라는 패키지가 있다. 해당 패키지에 있는 클래스들을 사용하려면 JVM에 로딩되어 있는 클래스와 메서드 정보를 읽어 올 수 있다.

## Class 클래스

class 클래스는 클래스에 대한 정보를 얻을 때 사용하기 좋다.

- String getName(): 클래스의 이름을 리턴(패키지 정보까지 리턴)
    - String getSimpleName(): 클래스 이름만 리턴
- Package getPackage(): 클래스의 패키지 정보를 패키지 클래스 타입으로 리턴한다.
- Field[] getFields(): public 으로 선언된 변수 목록을 Field 클래스 배열 타입으로 리턴
- Field getField(String name): public 으로 선언된 변수를 Field 클래스 타입으로 리턴한다.
- Field[] getDeclaredField(String name): name과 동일한 이름으로 정의된 변수를 Field 클래스 타입으로 리턴한다.
- Method[] getMethods(): public 으로 선언된 모든 메소드 목록을 Method 클래스 배열 타입으로 리턴한다. 해당 클래스에서 사용 가능한 상속받은 메소드도 포함된다.
- Method getMethod(String name, Class …parameterTypes): 지정된 이름과 매개변수 타입을 갖는 메소드를 Method 클래스 타입으로 리턴한다.
- Method[] getDeclareMethods(): 해당 클래스에서 선언된 모든 메소드 정보를 리턴한다.
- Method getDeclareMethod(String name, Class …parameterTypes): 지정된 이름과 매개변수 타입을 갖는 해당 클래스에서 선언된 메서드를 Method 클래스 타입으로 리턴한다.
- Constructor[] getConstructors(): 해당 클래스에 선언된 모든 public 생성자의 정보를 Constructor 배열 타입으로 리턴한다.
- Constructor[] getDeclaredConstructors(): 해당 클래스에서 선언된 모든 생성자의 정보를 Constructor 배열 타입으로 리턴한다.
- int getModifiers(): 해당 클래스의 접근자(modifier)정보를 int 타입으로 리턴한다.
- String toString(): 해당 클래스 객체를 문자열로 리턴한다.

## Method 클래스

- Class<?> getDeclaringClass(): 해당 메서드가 선언된 클래스 정보를 리턴한다.
- Class<?> getReturnType(): 해당 메서드의 리턴 타입을 리턴한다.
- Class<?>[] getParameterTypes(): 해당 메서드를 사용하기 위한 매개변수의 타입들을 리턴한다.
- String getName(): 해당 메서드의 이름을 리턴한다.
- int getModifiers(): 해당 메서드의 접근자 정보를 리턴한다.
- Class<?>[] getExceptionTypes(): 해당 메서드에 정의되어 있는 예외 타입들을 리턴한다.
- Object invoke(Object obj, Object …args): 해당 메서드를 수행한다.
- String toGenericString(): 타입 매개변수를 포함한 해당 메서드의 정보를 리턴한다.
- String toString(): 해당 메서드의 정보를 리턴한다.

## Field 클래스

Field 클래스는 클래스에 있는 변수들의 정보를 제공하기 위해서 사용한다.

- int getModifiers(): 해당 변수의 접근자 정보를 리턴한다.
- String getName(): 해당 변수의 이름을 리턴한다.
- String toString(): 해당 변수의 정보를 리턴한다.

# Reflection 클래스 잘못 사용 사례

```java
if (obj.getClass().getName().equals("java.math.BigDecimal") {
}
```

객체의 이름을 알아내기 위해서 getClass().getName() 메소드를 호출하여 사용했다. 이러한 코드가 응답 속도에 큰 영향을 주는 것은 아니지만, 어찌됐든 불필요한 작업을 하고 있는건 확실하다.

이러한 부분은 다음과 같이 개선 할 수 있다.

```java
if (obj instanceof java.math.BigDecimal) {
}
```

> 클래스의 메타 데이터 정보는 JVM의 Perm 영역에 저장된다.
만약 Class 클래스를 사용하여 많은 클래스들을 동적으로 생성하면 Perm 영역이 더 이상 사용할 수 없게 되어 OutOfMemoryError가 발생할 수 있다.
>