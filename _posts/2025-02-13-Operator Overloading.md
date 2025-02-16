---
layout: single
title: "Operator Overloading"
categories: C++ Basic
---

# 연산자 오버로딩

연산자 오버로딩은 +, =, *, <, == 등과 같은 연산자를 사용자 정의 타입(예, 클래스)에 대해 재정의하여, 기본 타입처럼 자연스럽게 사용할 수 있도록 하는 기능이다. 이를 통해 코드의 가독성과 직관성을 높일 수 있으나, 항상 사용해야 하는 것은 아니며 상황에 따라 신중히 적용해야 한다.

## 1. 연산자 오버로딩 개요

### 1.1 연산자 오버로딩이란?
- 사용자 정의 타입에 대해 +, -, *, =, <, == 등 연산자를 재정의할 수 있도록 한다.
- 이를 통해 클래스 객체를 기본 타입처럼 다룰 수 있으며, 예를 들어 `player1 + player2`와 같이 사용할 수 있다.
- 단, 대입 연산자(=)는 자동 생성되지만, 그 외의 연산자들은 사용자가 직접 구현해야 한다.

### 1.2 연산자 오버로딩의 사용 목적
- 사용자 정의 타입(예, `Point`, `Number`)을 기본 타입(int, float 등)처럼 자연스럽게 연산할 수 있도록 한다.
- 코드의 직관성과 가독성을 향상시킨다.
- 예시 (개념):
  ```cpp
  Point p1{10, 20};
  Point p2{30, 40};

  Point p3 = p1 + p2;     // 연산자 오버로딩을 통해 p1과 p2의 합을 구함

  cout << (p1 < p2) << endl;   // 비교 연산자 오버로딩
  cout << (p1 == p2) << endl;  // 동등 비교 오버로딩

  cout << p3 << endl;  // 출력 연산자 오버로딩 (추가 구현 필요)
  ```
  현재 기본 클래스에는 이러한 연산자가 정의되어 있지 않지만, 오버로딩을 통해 이러한 연산을 지원할 수 있다.

### 1.3 연산자 오버로딩의 기본 규칙
- **연산자 우선순위**는 변경할 수 없다.
- 단항, 이항, 삼항 연산자의 개념 자체는 변경 불가능하다.
- 기본 타입(int, float 등)에 대한 연산자는 오버로딩할 수 없다.
- 새로운 연산자를 정의할 수 없다.
- 연산자는 **멤버 함수 또는 전역 함수**로 오버로딩할 수 있다.
  - 단, `[]`, `()`, `->`, `=` 등 몇몇 연산자는 멤버 함수로만 오버로딩 가능하다.

## 2. 멤버 함수로서의 연산자 오버로딩

연산자 오버로딩은 멤버 함수로 구현하거나 전역 함수로 구현할 수 있다. 여기서는 주로 멤버 함수 형태의 예제를 살펴본다.

### 2.1 이항 연산자 오버로딩의 선언 형태
멤버 함수로 오버로딩할 경우 보통 아래와 같이 선언한다:
```cpp
Point operator+(const Point& rhs) const;
bool operator==(const Point& rhs) const;
bool operator<(const Point& rhs) const;
```
- 오른쪽 피연산자(`rhs`)는 const 참조로 전달하여 불필요한 복사를 피하고, 함수 내부에서는 수정하지 않음을 보장한다.
- 함수 자체를 const로 선언하여 왼쪽 피연산자(자기 자신)의 값이 변경되지 않음을 보장한다.

### 2.2 + 연산자 오버로딩 예제
```cpp
#include <iostream>
using namespace std;

class Point
{
private:
    int xpos;
    int ypos;
public:
    Point(int x = 0, int y = 0)
        : xpos{ x }, ypos{ y }
    { }
    
    void ShowPosition() const
    {
        cout << "[" << xpos << "," << ypos << "]" << endl;
    }
    
    // + 연산자 오버로딩: 두 점의 좌표를 더한 새 Point 객체 반환
    Point operator+(const Point& p) const
    {
        return Point{ xpos + p.xpos, ypos + p.ypos };
    }
};

int main()
{
    Point p1{ 10, 20 };
    Point p2{ 30, 40 };

    Point p3 = p1 + p2;  // 내부적으로 p1.operator+(p2)가 호출됨

    p3.ShowPosition();   // 출력: [40,60]
}
```

### 2.3 비교 연산자 오버로딩 예제
```cpp
#include <iostream>
using namespace std;

class Point
{
private:
    int xpos;
    int ypos;
public:
    Point(int x = 0, int y = 0)
        : xpos{ x }, ypos{ y }
    { }
    
    void ShowPosition() const
    {
        cout << "[" << xpos << "," << ypos << "]" << endl;
    }
    
    // + 연산자 오버로딩 (앞서 설명한 내용)
    Point operator+(const Point& p) const
    {
        return Point{ xpos + p.xpos, ypos + p.ypos };
    }
    
    // == 연산자 오버로딩: 두 점의 좌표가 동일하면 true 반환
    bool operator==(const Point& p) const
    {
        return (xpos == p.xpos) && (ypos == p.ypos);
    }
};

int main()
{
    Point p1{ 10, 20 };
    Point p2{ 10, 40 };
    Point p3 = p1 + p2;

    p3.ShowPosition();

    if (p1 == p2)  // p1.operator==(p2) 호출
        cout << "Same!" << endl;
    else
        cout << "Different!" << endl;
}
```

#### 2.4 단항 연산자의 오버로딩의 선언 형태
```cpp
Point operator-() const;
Point operator++();    // 전위 증가
Point operator++(int); // 후위 증가
bool operator!() const;
```
예시:
```cpp
Point p1{10,20};
Point p2 = -p1; // p1.operator-() 호출
```

#### 2.5 - (단항 부정) 연산자 오버로딩 예제
```cpp
#include <iostream>
using namespace std;

class Point
{
private:
    int xpos;
    int ypos;
public:
    Point(int x = 0, int y = 0)
        : xpos{ x }, ypos{ y }
    { }
    
    void ShowPosition() const
    {
        cout << "[" << xpos << "," << ypos << "]" << endl;
    }
    
    // 단항 부정 연산자 오버로딩: 좌표의 부호를 반전시킨 새 Point 객체 반환
    Point operator-() const
    {
        return Point{ -xpos, -ypos };
    }
};

int main()
{
    Point p1{ 10, 20 };
    Point p3 = -p1; // p1.operator-() 호출
    p3.ShowPosition(); // 출력: [-10,-20]
}
```

#### 2.6 멤버 함수로 선언 시 한계점
- 멤버 함수 오버로딩은 좌변의 객체를 기준으로 호출되므로, 교환법칙(Commutativity)을 완벽하게 보장하기 어려울 수 있다.
- 예를 들어, `p1 * 3`은 p1의 멤버 함수로 구현할 수 있지만, `3 * p1`은 불가능하다.
  ```cpp
  Point p1{10, 20};
  Point p2 = p1 * 3;   // p1.operator*(3)
  Point p3 = 3 * p1;   // 컴파일 오류: int에는 operator*가 정의되어 있지 않음
  ```

## 3. 전역 함수로서의 연산자 오버로딩

전역 함수로 오버로딩하면 좌변(lhs)과 우변(rhs) 모두를 매개변수로 받아 교환법칙을 보장할 수 있다. 단, private 멤버에 접근하기 위해 friend 선언을 사용하는 것이 일반적이다.

### 3.1 이항 연산자의 전역 함수 선언 예제
```cpp
Point operator+(const Point& lhs, const Point& rhs);
Point operator-(const Point& lhs, const Point& rhs);
bool operator==(const Point& lhs, const Point& rhs);
bool operator<(const Point& lhs, const Point& rhs);
```
사용 예:
```cpp
Point p1{ 10, 20 };
Point p2{ 30, 40 };
Point p3 = p1 + p2; // operator+(p1, p2) 호출
p3 = p1 - p2;       // operator-(p1, p2) 호출
```

### 3.2 전역 함수 + 연산자 오버로딩 (friend 사용)
```cpp
class Point
{
private:
    int xpos;
    int ypos;
public:
    Point(int x = 0, int y = 0)
        : xpos{ x }, ypos{ y }
    { }
    
    friend Point operator+(const Point& lhs, const Point& rhs)
    {
        return Point{ lhs.xpos + rhs.xpos, lhs.ypos + rhs.ypos };
    }
};
```
- 이 전역 함수는 Point 클래스의 멤버 함수가 아니므로, xpos와 ypos에 접근하기 위해 friend 선언을 사용한다.

### 3.3 * (곱셈) 연산자 오버로딩 예제
```cpp
// 멤버 함수로서 * 연산자 오버로딩: Point 객체에 scale을 적용
Point Point::operator*(int scale) const
{
    return Point{ xpos * scale, ypos * scale };
}

// 전역 함수로서 * 연산자 오버로딩: int * Point도 가능하도록 friend 함수로 구현
friend Point operator*(int scale, const Point& rhs)
{
    return rhs * scale; // 멤버 함수 operator*() 호출
}
```

## 4. 스트림 삽입 및 추출 연산자 오버로딩

### 4.1 구현 목적
- `<<` 연산자(삽입)와 `>>` 연산자(추출)를 오버로딩하여, 객체를 기본 자료형처럼 스트림에 출력하거나 입력받을 수 있도록 한다.
- 연쇄적 사용(예: `cout << p1 << p2;`)을 위해 ostream/istream 참조를 반환해야 한다.

### 4.2 cout에 대한 이해 예제
```cpp
class MyOstream
{
public:
    void operator<<(int val)
    {
        printf("%d", val);
    }
};

int main()
{
    cout << 123;          // cout.operator<<(123)
    // MyOstream 사용 예:
    MyOstream mycout;
    mycout << 123;        // mycout.operator<<(123)
    return 0;
}
```

### 4.3 스트림 삽입 연산자 오버로딩 구현
```cpp
#include <iostream>
using namespace std;

class Point
{
private:
    int xpos;
    int ypos;
public:
    Point(int x = 0, int y = 0)
        : xpos{ x }, ypos{ y }
    { }
    
    friend ostream& operator<<(ostream& os, const Point& rhs)
    {
        os << "[" << rhs.xpos << "," << rhs.ypos << "]";
        return os;
    }
    
    friend istream& operator>>(istream& is, Point& rhs)
    {
        int x, y;
        is >> x >> y;
        rhs = Point{ x, y };
        return is;
    }
};

int main()
{
    Point p1{ 10, 20 };
    cout << p1 << endl;  // 출력: [10,20]
    
    Point p2;
    cout << "Enter two integers: ";
    cin >> p2;
    cout << "You entered: " << p2 << endl;
}
```

## 최종 정리
- **연산자 오버로딩**을 통해 사용자 정의 타입을 기본 타입처럼 다룰 수 있다.
- 멤버 함수와 전역 함수(또는 friend 함수)로 오버로딩할 수 있으며, 전역 함수 방식을 사용하면 교환법칙을 보장하기 쉽다.
- 오버로딩할 때는 오른쪽 피연산자는 const 참조로 받고, 함수 자체를 const로 선언하여 객체의 불변성을 유지하는 것이 일반적이다.
- 일부 연산자(예: `[]`, `()`, `->`, `=` 등)는 멤버 함수로만 오버로딩 가능하다.
- 스트림 삽입(`<<`) 및 추출(`>>`) 연산자는 반드시 ostream/istream 참조를 반환하여 연쇄적인 사용이 가능하도록 구현해야 한다.

### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했음.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_
