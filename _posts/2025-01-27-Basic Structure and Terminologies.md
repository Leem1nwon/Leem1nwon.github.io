---
layout: single
title: "Basic Structure, Terminology, Memory and Variable"
---

---
## C++ keyword, Identifier, Operator

#### 키워드
- 약 90개의 키워드 존재 (변수 타입, if, for 등)
- 언어 자체에서 예약된 단어들

#### 식별자
- 변수, 함수, 타입 등 개발자가 지정하는 부분
- 대소문자를 구분함.
- 가독성 좋은 코드를 작성하기 위해서는, 식별자 이름을 의미있게 만들어야 함.

#### 연산자
- +, -, *, /, >>, <<, ::, ...

---
## C++ preprocessor
전처리기
- 컴파일 이전에 처리됨
- "#" 으로 시작
```
//include
#include <iostream>
#include "myFile.h"

// define
#ifdef
#ifndef
#define
#undef

#pragma
```

#include ?
- 단순한 복사 붙여넣기와 같다.
- #include <iostream>의 경우, iostream의 코드 내용을 복사 붙여넣기 한 것과 동일

#define ?
- Platform independecy 구현, 코드 단축, debug 목적 등으로 다양하게 활용

---

## main() function
모든 C++ 프로그램은 하나의 main()함수를 가져야 함
- main() 함수는 프로그램의 진입점(=프로그램이 실행되면 가장 먼저 실행되는 함수)
- 리턴값이 0이 올바른 프로그램 실행을 의미함

```
int main() {
    //code
}
```

---

## C++ namespace
**std**::cout

충돌 방지를 위해 사용
- 외부 라이브러리와 구현한 소스 코드간의 이름 충돌 가능성
- 또는 코드가 커지면, 내가 만든 함수들 사이에서도 실수로 충돌 가능

코드의 "그룹화"로 이해
- 서로다른 namespace로 그룹화하여 충돌을 방지할 수 있음.

![namespace](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/namespace.png)

"**::**" : scope resolution operator

#### using namespace
- 특정 namespace내의 함수들을 사용하겠다는 선언. **namespace::** 를 사용하지 않아도 됨
- 남용은 금물!
  - Using namespace를 모든 코드에 넣는다면, namespace의 기능을 상실한다.
  - 어느 namespace에 속하는 어떤 함수를 사용하겠다는 걸 명시하는 의미가 있기 때문
  
```
#include <iostream>

int main() {
    int favoriteNumber;

    std::cout << "Enter ...";
    std::cin >> favoriteNumber;
    std::cout << "Amazing!! ..." << std::endl;
}
```

---
## Basic I/O, Input
![stream](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/stream.png){width=500px}
#### cout과 <<
- C++의 표준 출력 스트림, 삽입 연산자
- 순차적인 출력이 가능

```
int age = 20;
std::cout << "My age is" << age;
```

- 줄바꿈이 필요한 경우 명시해 주어야 함.
```
std::cout << "My age is" << age << endl;
std::cout << "My age is" << age << "\n";
```

#### cin과 >>
- C++ 의 표준 입력 스트림, 추출 연산자
- 순차적인 입력이 가능
```  
cin >> myAge >> myHeight;
```
---
## 변수와 메모리
#### Bit, Byte, Hex
- 0.5 byte = 4 bit = $2^4$= 16개의 숫자 표현 가능 (양수 범위 0~15)
- 1 byte = 8 bit = $2^8$ = 256개의 숫자 표현 가능 (양수 범위 0~255)
- Hexadecimal(Hex, 16진수)
  - 16진수 2개로 1 byte 를 보기 쉽게 표현 &larr; hex 사용 이유
  - Hex 표현임을 알리기 위해 앞에 0x를 붙임
  
![images](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/hex.png){width=500px}

#### Memory

프로그래밍을 할 때 이용하는 가장 중요한 자원!

메모리는 읽고 쓸 수 있는 **바이트**공간의 집합
- 메모리에는 다양한 종류가 있으나, 여기서는 메인 메모리(RAM)를 지칭
- 층 하나(byte)마다 8개의 방(bit)이 있는 커다란 호텔이라고 생각
  - 각 방은 0 또는 1인 상태
  - 우리는 방(bit)단위로는 생각할 필요가 없음, 따라서 보기좋게 Hex라고 표현함

각 층에는 번호가 붙어있고, 이를 **주소**라고 함.
- 주소 또한 Hex로 표현함

|type|char|short|int|
|:--:|:--:|:--:|:--:|
|용량|1 byte|2 byte| 4 byte|
|10 저장 시|0x0A|0x000A|0x0000000A|

#### Variable

##### 변수란?
- **메모리의 주소에 붙이는 이름**
- 변수를 만들면, 메모리에 변수를 위한 공간(byte)이 확보됨
  - 공간의 크기(몇 칸)는 변수의 타입에 따라 결정
- 변수에 값을 대입하면, 그 메모리 주소에 값이 기록됨
- 변수를 만든다 &rarr; **변수를 정의**한다고 표현

변수라는 개념이 없다면
- 다음과 같이 불편하게 코드를 작성해야 했을 것이다.
``` 
int 0xFA84 = 10;
0xFA84 = 0xFA90 +6;
```
![images](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/variable%20and%20memory.png){width=400px}

##### 변수의 정의
(C++에서) 변수를 정의할 때는 반드시 타입을 명시해야 함
&rarr; 바이트를 몇 칸이나 확보해야 하는지 미리 알아야 하기 때문
```
char a;
int age;
double rate;
std::string name;
```
##### 변수의 초기화
변수를 정의하면서 초기값을 설정하는 것을 초기화라고 함
&rarr; 변수를 생성하는 시점부터, 메모리에 값이 저장되어 있음
```
char a = 10;
int age = 21;
double rate = 0.85;
std::string name = "Hyungki Kim";
```

##### Using a Variable
변수의 사용
- 변수 이름은 변수가 확보한 그 메모리에 접근하기 위해 사용됨
- 메모리에 값을 **읽고 쓰는 것**이 변수의 사용임
```
a = 20;          // 메모리 내 a 변수 위치에 20을 쓰기
printf("%d", a); // 메모리 내 a 변수 위치의 값을 읽어서 출력
```

간단한 규칙
- 이름 앞에 타입(int, float)이 붙어 있다? &rarr; 변수의 정의(초기화)
- 이름 앞에 아무것도 없다? &rarr; 변수의 사용

---

## 변수의 타입

#### Integer
정수를 표현 
![images](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/integer.png){width=600px}

#### Floating Point
실수를 표현
![images](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/floating%20poin.png){width=600px}

#### Boolean
true / false
![images](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/boolean.png){width=600px}

#### sizeof 연산자
타입 도는 변수의 바이트 단위 크기를 리턴
```
sizeof(int) // 타입의 크기
sizeof(double) // 타입의 크기
sizeof(favoriteNumber) // 변수의 크기
```
---
## 상수
#### 상수란?
- 변수와 유사하지만, 초기화 이후 변할 수 없는 값이다.
- 사용 목적 &rarr; **실수 방지, 프로그램의 견고함**
- 실제 현장에서, 그리고 다른 사람과 협업을 할 때 매우 중요한 매너!

#### 상수의 종류
- 리터럴 상수 : 12, 3.14, "Minwon"
- 선언 상수 : const in favoriteNumber = 18;
- 상수 표현 : constexpr int age = 20;
- 열거형
- Defined : #define pi 3.1415926

---

### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_

