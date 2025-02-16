---
layout: single
title: "Generic Programming and Template"
categories: Basic_C++
---

# 제네릭 프로그래밍과 템플릿

## 1. 제네릭 프로그래밍

### 1.1 개요
- **정의**: 타입에 관계없이 동작하는 '일반적인' 코드를 작성하는 방법.
- 예를 들어, 여러 가지 덧셈 함수를 구현할 때  
  - 기존에는 `add(int, int)`, `add(double, double)`, `add(Point, Point)` 등 각각 따로 구현해야 했다.
- 제네릭 프로그래밍 기법을 사용하면 한 번의 코드로 다양한 타입에 대해 동작하는 함수를 만들 수 있다.

### 1.2 제네릭 프로그래밍의 구현 방법
- **매크로 사용** ⟵ 주의가 필요함 (단순 치환 방식)
- **함수/클래스 템플릿 사용**

### 1.3 매크로를 사용한 제네릭 프로그래밍
**매크로 (#define)**  
- 코드를 단순 대체(복붙)하는 방식  
```cpp
#define MAX_SIZE 100
#define PI 3.14159

if (num > MAX_SIZE)
    cout << "Too big" << endl;

double area = PI * r * r;
```

### 1.4 Generic 하지 않은 프로그래밍
- 예를 들어, int 타입에 대한 Max 함수를 구현하면:
```cpp
int Max(int a, int b)
{
    return (a > b) ? a : b;
}

int x = 100;
int y = 200;
cout << Max(x, y);   // 200
```
- double, char 등 다른 타입으로 확장하려면 추가 구현이 필요하다.
```cpp
double Max(double a, double b)
{
    return (a > b) ? a : b;
}

char Max(char a, char b)
{
    return (a > b) ? a : b;
}
```

### 1.5 매크로를 사용한 max 함수의 일반적인 구현
- 매크로는 인수(a, b)를 받아 타입에 구애받지 않고 generic하게 코드를 작성할 수 있다.
```cpp
#define Max(a, b) ((a > b) ? a : b)

cout << Max(10, 20) << endl;     // 20
cout << Max(2.4, 3.5) << endl;   // 3.5
cout << Max('A', 'C') << endl;   // 'C'
```

### 1.6 매크로 사용 시 유의사항
- 코드는 단순 치환되므로, 괄호로 감싸는 것이 안전하다.
  - 세미콜론(;)은 포함하지 않는 것이 기본 약속.
- 매크로는 디버깅이 어렵고, 예상치 못한 에러가 발생할 수 있음.
```cpp
#define SQUARE(a) a * a   // 위험: SQUARE(5 + 1) → 5 + 1 * 5 + 1 = 5 + 5 + 1
// 안전하게 구현하려면:
#define SQUARE(a) ((a) * (a))

result = SQUARE(5);      // 25
result = 100 / SQUARE(5);  // 올바른 결과: 100 / ((5) * (5)) = 4
```

## 2. Generic Programming using Template

### 2.1 C++ 템플릿
- **템플릿**은 클래스나 함수에 대한 '설계도'로, 데이터 타입에 관계없이 동작하는 코드를 작성할 수 있게 해준다.
- 컴파일러는 템플릿 코드를 실제 사용되는 타입에 맞게 생성하며, 사용되지 않으면 코드를 생성하지 않는다.
- 함수 템플릿과 클래스 템플릿 모두 지원된다.

### 2.2 템플릿을 사용한 max 함수의 구현
- 템플릿을 이용하면 타입 이름을 T로 대체하여 하나의 함수로 여러 타입을 지원할 수 있다.
```cpp
template <typename T>
T Max(T a, T b)
{
    return (a > b) ? a : b;
}
```
- 템플릿 함수는 자료형을 명시할 수 있으며, 컴파일러가 타입을 추론할 수 있으면 생략 가능하다.
```cpp
double c = 1.23, d = 4.56;
cout << Max<double>(c, d);
cout << Max(c, d);  // 타입 추론 가능
```

### 2.3 템플릿 사용 조건
- 템플릿 함수(예: max)는 내부에서 사용하는 연산자(예: >)가 해당 타입에 대해 정의되어 있어야 한다.
- 예를 들어, `Point` 클래스에서 Max 함수를 사용하려면, Point 클래스에 > 연산자가 오버로딩되어 있어야 한다.
```cpp
Point p1{10, 20};
Point p2{20, 30};

cout << Max<Point>(p1, p2);
```

### 2.4 템플릿의 다중 매개변수
- 템플릿은 여러 매개변수를 사용할 수 있으며, 서로 다른 이름을 부여해 각기 다른 타입을 받을 수 있다.
```cpp
template <typename T1, typename T2>
void func(T1 a, T2 b)
{
    cout << a << " " << b;
}

func<int, double>(10, 20.05);
func('A', 12.5);  // 타입 추론 가능
```

### 2.5 템플릿의 특수화
- 특정 자료형에 대해 별도의 구현을 제공할 수 있다.
```cpp
template <typename T>
T Min(T a, T b) {
    return (a < b) ? a : b;
}

// std::string에 대한 명시적 특수화
template <>
std::string Min(std::string a, std::string b)
{
    return (a.length() < b.length()) ? a : b;
}
```

## 3. 클래스 템플릿

### 3.1 클래스 템플릿이란
- 클래스 템플릿은 함수 템플릿과 유사하게, 특정 타입에 대해 동작하는 클래스를 생성할 수 있는 설계도이다.
- 컴파일러는 템플릿 인자에 따라 실제 클래스를 생성한다.

### 3.2 Item 클래스 예제
일반 클래스:
```cpp
class Item
{
private:
    string name;
    int value;
public:
    Item(string name, int value)
        : name{name}, value{value} { }
    string getName() const { return name; }
    int getValue() const { return value; }
};
```
템플릿 클래스:
```cpp
template <typename T>
class Item
{
private:
    string name;
    T value;
public:
    Item(string name, T value)
        : name{name}, value{value} { }
    string getName() const { return name; }
    T getValue() const { return value; }
};

int main()
{
    Item<int> item1{"Kim", 1};
    Item<double> item2{"Lee", 10.5};
    Item<string> item3{"Park", "Hello"};
}
```

### 3.3 클래스 템플릿의 다중 매개변수
```cpp
template <typename T1, typename T2>
class MyPair {
private:
    T1 first;
    T2 second;
public:
    MyPair(T1 val1, T2 val2)
        : first{val1}, second{val2} { }
};

int main()
{
    MyPair<string, int> p1{"Kim", 1};
    MyPair<int, double> p2{123, 45.6};
}
```

### 3.4 클래스 템플릿의 (부분)특수화
- 클래스 템플릿의 일부 인자에 대해 별도의 구현을 제공할 수 있다.
```cpp
template <typename T1, typename T2>
class Item {
private:
    T1 key;
    T2 value;
public:
    // 일반적인 구현
};

template <typename T>
class Item<T, double> {  // value가 double인 경우의 특수화
private:
    T key;
    double value;
public:
    // 특수화된 구현
};
```

### 3.5 클래스 템플릿의 매개변수 (non-type)
- 템플릿은 타입이 아닌 다른 값도 인자로 사용할 수 있다.
```cpp
template <typename T, int N>
class Array 
{
private:
    int size = N;
    T values[N];
    // ...
};

int main() {
    Array<int, 5> nums;      // Array<int, 5>가 생성됨
    Array<double, 10> nums2;
}
```

## 4. 정리
- **Generic Programming**이란 타입에 관계없이 작동하는 코드를 작성하는 방법이다.
  - 매크로를 활용하는 방식은 단순 치환 방식으로 구현되지만, 디버깅 및 유지보수에 어려움이 있다.
- **템플릿을 사용한 Generic Programming**은 컴파일 시 실제 사용되는 타입에 맞게 코드가 생성된다.
  - 함수 템플릿과 클래스 템플릿을 통해 다양한 타입을 지원하며, non-type 템플릿 인자도 사용할 수 있다.

### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했음.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_
