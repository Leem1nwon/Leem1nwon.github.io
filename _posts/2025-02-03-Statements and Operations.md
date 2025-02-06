---
layout: single
title: "Statements and Operations"
---

## Expressions & Statements

#### 표현식
코드의 가장 작은 구성요소

```cpp
13                  // literal
favorite_number     // variable
3 + 5               // addition
a < b               // relational
a = 20              // assignment
```

#### 명령문
- 명령을 수행하는 코드 단위
- 세미콜론 (;) 으로 끝나는 문장
- 표현식의 집합

```cpp
int favorite_number;                            // declaration
favorie_number = 20;                            // assignment
3 + 5;                                          // expression
favorite_number = 3 * 5;                        // assignment
if (a>b) std::cout << "a is greater than b";    // if
```

---

## Operations

#### 연산자 개수에 따른 분류류
단항(unary), 이항(binary), 삼항(ternary) 연산자

#### 대입 연산자
- lhs = rhs
- l-value & r-value
- 오른쪽의 값을 계산하여 왼쪽에 대입
- 컴파일러가 대입이 가능한지 체크함
  
```cpp
int num1 = 0;
num1 = "Minwon" // Compiler Error!
```
- 왼쪽은 대입이 가능해야 함 (리터럴, 상수는 될 수 없음)

#### 산술 연산자
- +, -, *, /, %

#### 증감 연산자
- Prefix (++num, --num) : 대입 전 증감
- Postfix (num++, num--) : 대입 후 증감

#### 비교 연산자
- ==, !=
- 결과는 Boolean 타입의 true or false

#### 관계 연산자
- <, >, <=, >=

#### 논리 연산자
- !, &&, || (not, and, or)
- short-circuit evaluation : 결과 파악이 가능한 경우, 나머지 연산을 하지 않음 (최적화)

```cpp
if(expr1 && expr2 && expr3)   // if expr1 is false? -> 뒤에는 안봄
if(expr1 || expr2 || expr3)   // if expr1 is true? -> 뒤에는 안봄
```

#### 복합 연산자

![복합연산자](../images/2025-02-03-Statements%20and%20Operations/복합연산자.png){width=700px}

---
### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_