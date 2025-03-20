---
layout: single
title: "Reference"
categories: Basic_C++
---

# 참조자

## 1. 참조자란

### 1-1. 개념
- 변수의 "별명"이다.
- 참조자는 새로운 변수가 아니다.  
  - 즉, 메모리에 새로운 공간을 차지하지 않는다.
  - 포인터는 그 자체로 주소값을 저장하지만, 참조자는 기존 변수의 별명으로 동작한다.

### 1-2. 참조자 특징
- **선언과 동시에 초기화되어야 함** (null 상태가 될 수 없음)
- **한 번 초기화되면 다른 변수의 참조자가 될 수 없음**
- const 포인터처럼 동작하면서, 사용 시 자동으로 역참조된다.
  - 포인터보다 간단하고 편리한 버전으로 볼 수 있음.
- C++에서 함수의 매개변수로 매우 자주 사용된다.

### 1-3. 정의
- `int&`, `float&` 등의 형식으로, 변수 정의 시 `&`를 붙여 참조자를 생성한다.
- 포인터와 마찬가지로, 동일한 타입에 대해서만 참조자를 생성할 수 있다.
- 참조자는 그 자체가 변수처럼 사용된다.

```cpp
int main() { 
    int a = 10;
    int& b = a;   // a의 참조자 b 생성

    b = 20;       // b를 변경하면 a도 변경됨!
    std::cout << a << std::endl;    // 20 출력

    int& c;       // ERROR! 참조자는 선언과 동시에 초기화되어야 함.
}
```

```cpp
void Increment(int val) {
    val++;
}
/* 이 함수의 val은 a의 값을 복사해온 지역 변수이므로, a에 영향을 주지 않으며 함수 종료 시 사라짐 */

void IncrementByReference(int& val) {
    val++;
}
/* int& val은 a의 참조자이므로, 함수 내에서 a의 값을 직접 변경할 수 있음 */

int main() {
    int a = 5;
    Increment(a);
    std::cout << a << std::endl;    // 5 출력

    IncrementByReference(a);
    std::cout << a << std::endl;    // 6 출력

    return 0;
}
```

### 1-4. const와 참조자 사용
- 복사가 필요하지 않은 경우는 **const 참조자 (const T&)**를 사용한다.
  - 이는 값의 불필요한 변경을 방지하고, 메모리 복사를 줄이며, r-value 전달도 가능하게 한다.
  - 예: `PrintConstRef(10)`처럼 리터럴도 전달 가능하다.

### 1-5. 예시
```cpp
void PrintConstRef(const int& val) {
    std::cout << val << std::endl;
}

void PrintAddress(int* valPtr) {
    std::cout << *valPtr << std::endl;
}

void PrintRef(int& val) {
    std::cout << val << std::endl;
}

void PrintVal(int val) {
    std::cout << val << std::endl;
}

int main() {
    int a = 5;
    PrintVal(a);
    PrintRef(a);
    PrintConstRef(a);
    PrintAddress(&a);
}
```

| 함수                                    | 메모리 복사 여부 | r-value 전달 가능 여부 | 값 변경 가능 여부 | 사용 사례                                       |
|-----------------------------------------|------------------|------------------------|------------------|------------------------------------------------|
| `void PrintVal(int val)`                | ✅ (복사됨)      | ✅ 가능                | ✅ 가능          | 작은 변수(`int`, `char`) 전달 시                |
| `void PrintRef(int& val)`               | ❌ (참조됨)      | ❌ 불가능              | ✅ 가능          | 값 변경이 필요한 경우                           |
| `void PrintConstRef(const int& val)`    | ❌ (참조됨)      | ✅ 가능                | ❌ 불가능        | 값 변경 없이 전달할 때 (가장 추천)               |
| `void PrintAddress(int* valPtr)`        | ❌ (참조됨)      | ❌ 불가능              | ✅ 가능          | 포인터를 사용해야 하는 경우 (주소 전달)          |

✅ **일반적으로 `const int&`를 가장 많이 사용한다.**  
✅ **포인터(`int*`)는 특별한 경우에만 사용한다.**  
✅ **작은 값(예: `int`, `char`)은 값 복사 방식으로도 괜찮다.**

--------------------------------------------------

## 2. l-value와 r-value

### 2-1. l-value
- 이름을 가지며, 메모리 주소가 있는 값.
- const가 아니라면 수정이 가능하다.
```cpp
int x = 100;            // x는 l-value
std::string name = "Bruno";  // name은 l-value
```

### 2-2. r-value
- 메모리 주소가 없으며, 대입의 좌측에 올 수 없는 값.
- 예: 리터럴, 대입식의 오른쪽 값 등.
```cpp
int x = 100;    // 100은 r-value
std::string name = "Rasmus";   // "Rasmus"는 r-value
int maxNum = std::max(20,30);  // max(20,30)은 r-value
```

### 2-3. 참조자와 r-value
- 기본적으로 l-value에 대해서만 참조자를 만들 수 있으나,
  - const 참조자는 r-value도 받을 수 있다.
  - C++11부터 r-value reference (`&&`)도 지원한다.
```cpp
int x = 100;

int& ref1 = x;
ref1 = 200;

int& ref2 = 100;         // ERROR: 100은 l-value가 아님
const int& ref3 = 100;   // 허용됨
int&& ref4 = 100;        // 허용됨 (r-value reference)
```

--------------------------------------------------

## 3. 포인터 vs 참조자

### 3-1. 간단 정리
- **'*'의 사용**
  - 변수 정의 시: 포인터 정의 (예: `int* a`)
  - 변수 사용 시: 포인터의 역참조 (예: `*a`)
- **'&'의 사용**
  - 변수 정의 시: 참조자 정의 (예: `int& b`)
  - 변수 사용 시: 변수의 주소 반환 (예: `&a`)

### 3-2. Pass-by-value
```cpp
void func(int a)
```
- 함수가 원본을 수정하지 않을 때 사용.
- `int`, `char`, `double` 등 작은 기본 자료형에 적합.
- 복사 비용이 큰 `std::string`, `std::vector`, 클래스 등은 지양.

### 3-3. Pass-by-address
```cpp
void func(int* a)
```
- 원본을 수정해야 하거나, 복사 비용이 클 때 사용.
- 포인터가 `nullptr`일 수 있으나, 참조자는 될 수 없다.

### 3-4. const 포인터를 사용한 Pass-by-address
```cpp
void func(const int* a)
```
- 함수 내에서 데이터를 수정하지 않을 때 사용.
- 복사 비용이 큰 경우 유용하며, 포인터가 `nullptr`이어도 괜찮다.

### 3-5. const와 const인 포인터를 사용한 Pass-by-address
```cpp
void func(const int* const a)
```
- 데이터와 포인터 모두 수정 불가능해야 할 때 사용.

### 3-6. Pass-by-reference (참조자 사용)
```cpp
void func(int& a)
```
- 함수 내에서 원본을 수정해야 할 때 사용.
- 참조자는 `nullptr`이 될 수 없으므로, 안전하게 사용 가능.

### 3-7. const 참조자를 사용한 Pass-by-const-reference
```cpp
void func(const int& a)
```
- 일반적으로 가장 많이 사용된다.
- 데이터 변경 없이 전달할 때, 복사 비용을 줄이기 위해 사용.

--------------------------------------------------

## 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했음.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_
