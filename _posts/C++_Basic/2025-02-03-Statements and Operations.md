---
layout: single
title: "Statements and Operations"
categories: Basic_C++
---

# C++ 프로그램의 구조

## 1. Expressions & Statements

### 1.1 표현식 (Expressions)
- **정의:**  
  코드의 가장 작은 구성요소.
- **예제:**
  ```cpp
  13                  // literal
  favorite_number     // variable
  3 + 5               // addition
  a < b               // relational
  a = 20              // assignment
  ```

### 1.2 명령문 (Statements)
- **정의:**  
  명령을 수행하는 코드 단위로, 세미콜론(;)으로 끝나는 문장이다.
- **특징:**  
  표현식의 집합으로 구성된다.
- **예제:**
  ```cpp
  int favorite_number;                            // declaration
  favorite_number = 20;                           // assignment
  3 + 5;                                          // expression
  favorite_number = 3 * 5;                        // assignment
  if (a > b) std::cout << "a is greater than b";  // if 문
  ```

--------------------------------------------------

## 2. Operations

### 2.1 연산자 분류
- **단항 (Unary)**
- **이항 (Binary)**
- **삼항 (Ternary)**

### 2.2 대입 연산자
- **형식:** `lhs = rhs`
- **동작:** 오른쪽의 값을 계산하여 왼쪽에 대입한다.
- **주의:** 왼쪽은 l-value여야 한다.
- **예제:**
  ```cpp
  int num1 = 0;
  num1 = "Minwon"; // Compiler Error! (문자열은 대입 불가)
  ```

### 2.3 산술 연산자
- **예:** `+`, `-`, `*`, `/`, `%`

### 2.4 증감 연산자
- **Prefix:** `++num`, `--num` (대입 전에 증감)
- **Postfix:** `num++`, `num--` (대입 후 증감)

### 2.5 비교 연산자
- **예:** `==`, `!=`
- **결과:** Boolean 값 (true 또는 false)

### 2.6 관계 연산자
- **예:** `<`, `>`, `<=`, `>=`

### 2.7 논리 연산자
- **예:** `!`, `&&`, `||`
- **특징:**  
  - Short-circuit evaluation:  
    예를 들어, `if(expr1 && expr2 && expr3)`에서 `expr1`이 false이면 나머지 평가하지 않음.
    `if(expr1 || expr2 || expr3)`에서 `expr1`이 true이면 나머지 평가하지 않음.

### 2.8 복합 연산자
- 복합 연산자는 여러 연산자를 결합한 형태를 의미한다.
- **예제 이미지:**  
  ![복합연산자](../images/2025-02-03-Statements%20and%20Operations/복합연산자.png){width=700px}

--------------------------------------------------

## 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_
