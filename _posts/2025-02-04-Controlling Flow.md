---
layout: single
title: "Controlling Flow"
categories: Basic_C++
---

# 제어문(조건문, 반복문)

이 문서에서는 C++의 제어문에 대해 다룹니다. 제어문은 조건문과 반복문으로 구분되며, 프로그램의 흐름을 제어하는 데 사용됩니다.

--------------------------------------------------

## 1. 조건문

조건문은 주어진 조건식이 참일 경우에만 특정 명령문들을 실행하는 코드 블록입니다.

### 1.1 if 문
- **설명:**  
  조건식이 참일 경우 단일 명령문 또는 블록을 실행한다.
- **예제:**
  ```cpp
  if (expr)
      statement;

  if(num > 10)
      ++num;
  ```

### 1.2 if문과 블록
- **설명:**  
  조건문에서 여러 명령문을 실행하고자 할 경우, 중괄호 `{}` 로 감싸서 하나의 블록으로 만들어야 한다.
- **예제:**
  ```cpp
  if(expr) {
      int a = 10; // 지역 변수: 이 블록 내에서만 유효
  }

  std::cout << a << std::endl; // Error: 'a'는 블록 밖에서 접근할 수 없음
  ```

### 1.3 if - else 문
- **설명:**  
  조건식의 결과에 따라 두 가지 분기 중 하나의 명령문 블록을 실행한다.
- **예제:**
  ```cpp
  if(expr) {
      statement1;
  }
  else {
      statement2;
  }
  ```

### 1.4 nested if 문
- **설명:**  
  조건문 내부에 또 다른 조건문을 작성하여 복잡한 조건을 처리할 수 있다.
- **예제:**
  ```cpp
  if (my_score != your_score) {
      if (my_score > your_score) {
          std::cout << "I win!";
      }
      else {
          std::cout << "You win!";
      }
  }
  else {
      std::cout << "Tie!";
  }
  ```

### 1.5 switch 문
- **설명:**  
  정수형 표현식을 기반으로 여러 경우(case)로 분기하는 조건문.
  - `switch`문의 표현식은 정수형이어야 하며, case 뒤의 값은 반드시 정수형 리터럴이어야 한다.
- **예제 (올바른 사용):**
  ```cpp
  int num = 2;
  switch (num) {
      case 1:
          printf("num은 1입니다.\n");
          break;
      case 2:
          printf("num은 2입니다.\n");
          break;
  }
  ```
- **예제 (잘못된 사용):**
  ```cpp
  int num = 2;
  int caseValue = 1;
  switch (num) {
      case caseValue:  // ERROR: case 뒤에는 정수형 리터럴만 가능
          printf("num은 1입니다.\n");
          break;
  }
  ```

### 1.6 삼항 연산자 (?:)
- **형식:**  
  `(conditional_expr) ? expr1 : expr2`
- **설명:**  
  조건식이 참이면 `expr1`의 값을, 거짓이면 `expr2`의 값을 반환한다.
- **용도:**  
  if-else 문을 한 줄로 간결하게 표현할 때 사용된다.

--------------------------------------------------

## 2. 반복문

반복문은 특정 조건이 참인 동안 또는 일정 횟수만큼 명령문을 반복해서 실행하는 데 사용된다.

### 2.1 for 문
- **형식:**  
  ```cpp
  for (초기화; 종료조건; 증감) {
      명령문;
  }
  ```
- **예제 (배열 루프):**
  ```cpp
  int scores[] = {100, 90, 50};
  
  for (int i = 0; i < 3; i++) {
      std::cout << scores[i] << std::endl;
  }
  ```
- **예제 (콤마 연산자 사용):**
  ```cpp
  for (int i = 0, j = 5; i < 5; i++, j++) {
      std::cout << i << " * " << j << " = " << i * j << std::endl;
  }
  ```
- **예제 (조건 생략):**
  ```cpp
  int i = 0;
  for (; true; ) {
      i++;
      if (i <= 5) {
          std::cout << i << std::endl;
      }
      else {
          break;
      }
  }
  ```

### 2.2 while 문
- **형식:**
  ```cpp
  while(expr) {
      statements;
  }
  ```
- **예제:**
  ```cpp
  int i = 0;
  while (i < 5) {
      std::cout << i << std::endl;
      i++;
  }
  ```
- **Boolean flag 사용 예:**
  ```cpp
  bool is_done = false;
  int number = 0;
  
  while (!is_done) {
      std::cout << "Enter number under 10" << std::endl;
      std::cin >> number;
      if (number >= 10) {
          std::cout << "Wrong number" << std::endl;
      }
      else {
          std::cout << "OK!" << std::endl;
          is_done = true;
      }
  }
  ```

### 2.3 do while 문
- **형식:**
  ```cpp
  do {
      statements;
  } while (expr);
  ```
- **특징:**  
  반복 조건을 나중에 검사하므로, 최소 한 번은 실행된다.

### 2.4 continue와 break
- **continue:**  
  현재 iteration의 남은 부분을 건너뛰고, 다음 iteration으로 넘어간다.
- **break:**  
  반복문 전체를 즉시 종료하고, 반복문 바깥으로 빠져나간다.

![continue와 break](../images/2025-02-05-Controlling%20Flow/continue_break.png)

--------------------------------------------------

## 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzh
