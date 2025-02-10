---
layout: single
title: "Reference"
---

# 참조자

## 1. 참조자란

#### 1-1. 개념
- 변수의 "별명"
- 참조자는 (새로운) 변수가 아님  
  - 즉, 메모리에 새로운 공간을 차지하지 않는다.
  - 포인터 변수는 그 자체로 주소값을 저장하는 변수인 반면, 참조자는 기존 변수의 별명이다.

#### 1-2. 참조자 특징
- **선언과 동시에 초기화되어야 함 (null일 수 없음)**
- **한 번 초기화되면 다른 변수의 참조자가 될 수 없음**
- const인 포인터처럼 동작하면서, 사용 시 자동으로 역참조되는 개념  
  - 포인터보다 간단하고 편리한 버전으로 생각할 수 있음.
- C++에서 함수의 매개변수로 매우 자주 사용됨.

#### 1-3. 정의

- `int&`, `float&` 등, 변수 정의 시에 `&`를 붙인 타입을 사용.
- 포인터와 마찬가지로, 동일한 타입에 대해서만 참조자 생성이 가능.
- 참조자를 사용할 때는, 마치 그 자체가 변수인 것처럼 그냥 사용하면 됨.

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
/* 이 함수의 val은 a의 값을 복사해온 지역 변수이므로, a에 영향을 주지 않고 함수 종료 시 사라짐 */

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

#### 1-4. const와 참조자 사용
- 복사가 필요한 경우가 아니라면, **const &**를 쓰는 것이 기본이다.
  - const로 인해 불필요한 값 변경을 방지할 수 있음.
  - 참조자 사용으로 불필요한 메모리 복사를 피할 수 있음.
  - r-value 전달이 가능해진다 (예: 10, 3+5와 같은 리터럴이나 표현식도 전달 가능).

#### 1-5. 예시
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

| 함수                             | 메모리 복사 여부 | r-value 전달 가능 여부 | 값 변경 가능 여부 | 사용 사례                                         |
|----------------------------------|------------------|------------------------|------------------|--------------------------------------------------|
| `void PrintVal(int val)`         | ✅ (복사됨)      | ✅ 가능                | ✅ 가능          | 작은 변수(`int`, `char`) 전달 시 사용             |
| `void PrintRef(int& val)`        | ❌ (참조됨)      | ❌ 불가능              | ✅ 가능          | **값을 변경할 필요가 있는 경우**                  |
| `void PrintConstRef(const int& val)` | ❌ (참조됨)   | ✅ 가능                | ❌ 불가능        | **값을 변경하지 않고 전달할 때 (가장 추천)**        |
| `void PrintAddress(int* valPtr)` | ❌ (참조됨)      | ❌ 불가능              | ✅ 가능          | **포인터를 사용해야 하는 경우**                   |

✅ **일반적으로 `const int&`를 가장 많이 사용한다.**  
✅ **포인터(`int*`)는 특별한 경우에만 사용한다.**  
✅ **작은 값(예: `int`, `char`)은 그냥 값 복사(`int val`)로도 괜찮다.**

---

## 2. l-value와 r-value

#### 2-1. l-value
- 이름이 있으며, 주소를 가지는 값.
- const가 아니라면 수정 가능한 값.

```cpp
int x = 100;           // x는 l-value

std::string name = "Bruno";   // name은 l-value
```

#### 2-2. r-value
- 주소를 갖지 않으며, 대입의 좌측에 올 수 없는 값.
- l-value가 아닌 것들:
  - 대입식의 오른쪽에 위치하는 값
  - 리터럴 등

```cpp
int x = 100;    // 100은 r-value

std::string name = "Rasmus";   // "Rasmus"는 r-value

int maxNum = std::max(20,30);   // max(20,30)은 r-value
```

#### 2-3. 참조자와 r-value
- 기본적으로는 l-value에 대해서만 참조자를 만들 수 있으나,
  - const 참조자의 경우는 r-value도 받을 수 있음.
  - 또한, C++11부터는 r-value reference 문법도 존재한다.

```cpp
int x = 100;

int& ref1 = x;
ref1 = 200;

int& ref2 = 100;         // ERROR! 100은 l-value가 아님
const int& ref3 = 100;   // 허용됨
int&& ref4 = 100;        // 허용됨 (r-value reference)
```

---

## 3. 포인터 vs 참조자

#### 3-1. 간단 정리

**'*'의 사용**
- 변수를 정의할 때 붙이면 → 포인터의 정의 (예: `int* a`)
- 변수를 사용할 때 붙이면 → 포인터의 역참조 (예: `*a`)

**'&'의 사용**
- 변수를 정의할 때 붙이면 → 참조자의 정의 (예: `int& b`)
- 변수를 사용할 때 붙이면 → 변수의 주소값 반환 (예: `&a`)

#### 3-2. Pass-by-value
```cpp
void func(int a)
```
- 함수가 실제 매개변수(원본)를 수정하지 않는 경우에 사용.
- `int`, `char`, `double` 등 크기가 작은 기본 자료형 전달 시 사용.  
  - `std::string`, `std::vector`, 클래스 등은 복사 비용이 크므로 지양함.

#### 3-3. Pass-by-address
```cpp
void func(int* a)
```
- 함수가 실제 매개변수(원본)를 수정해야 하는 경우에 사용.
  - 예를 들어, 지역 범위 밖의 메모리에 저장된 값에 접근해야 할 때.
- 복사 오버헤드가 클 때 사용.
- 포인터가 `nullptr`이 되어도 상관 있는 경우에 사용.  
  - 참조자는 `nullptr`이 될 수 없음.

#### 3-4. const 포인터를 사용한 Pass-by-address
```cpp
void func(const int* a)
```
- 함수가 실제 매개변수를 수정하지 않을 때 사용.
- 복사 오버헤드가 클 때 사용.
- 포인터가 `nullptr`이어도 상관 있음.  
  - 참조자는 `nullptr`이 될 수 없음.

#### 3-5. const와 const인 포인터를 사용한 Pass-by-address
```cpp
void func(const int* const a)
```
- 함수가 실제 매개변수를 수정하지 않을 때 사용.
- 복사 오버헤드가 클 때 사용.
- 포인터가 `nullptr`이어도 상관 있음.
- **포인터 자체가 바뀌지 않아야 할 때 사용.**

#### 3-6. Pass-by-reference (참조자 사용)
```cpp
void func(int& a)
```
- 함수가 실제 매개변수를 **수정**해야 할 때 사용.
- 복사 오버헤드가 클 때 사용.
- 매개변수가 **nullptr**이 될 수 없다는 것이 보장될 때 사용.

#### 3-7. const 참조자를 사용한 Pass-by-const-reference
```cpp
void func(const int& a)
```
- 가장 많이 사용됨.
- 함수가 실제 매개변수를 수정하지 않을 때 사용.
- 복사 오버헤드가 클 때 사용.
- 매개변수가 **nullptr**이 될 수 없다는 것이 보장될 때 사용.

---
### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했음.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_