---
layout: single
title: "Controlling Flow"
---
제어문(조건문, 반복문)

## 조건문

#### If 문 

표현식이 참일 경우에만 실행하는 명령문

```cpp
if (expr)
    statement;

if(num > 10)
    ++num;
```

#### if문과 블록
- 조건문 내에서 하나 이상의 명령문을 실행하기 위해서는, 블록 내에 명령문을 작성
- 블록은 "{"로 시작해서 "}"로 끝남
- 블록 내에서 선언된 변수는 **'지역변수'**라 하며, 블록 내에서만 접근 가능

```cpp
if(expr) {
    int a = 10; // local variable
}

std::cout << a << std::endl; // Error: 'a'는 이 블록 밖에서 접근할 수 없음
```

#### if - else 문
- 표현식의 참/거짓 여부에 따라 명령문 실행 분기
- else if 키워드를 통해 다양한 조건을 기술 가능
  
```cpp
if(expr) {
    statement1;
}
else {
    statement2;
}
```

#### nested if 문
- 블록과 if-else문을 중첩하여 복잡한 조건을 효율적으로 기술 가능

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

#### switch문
- switch, case, default를 사용한 분기문
- **switch 문의 표현식** → **정수형 값**이면 됨 (변수, 상수, 연산 결과 등 가능)
- **case 뒤의 값** → 반드시 **정수형 리터럴**이어야 함 (변수 불가)

✅ 올바른 사용 예시 (정수형 변수 가능)
```cpp
int num = 2;  // 정수형 값
switch (num) {  // 정수형 변수 사용 가능
    case 1:  // case에는 정수형 리터럴 필요
        printf("num은 1입니다.\n");
        break;
    case 2:
        printf("num은 2입니다.\n");
        break;
}
```

❌ 잘못된 사용 예시 (case 값에 변수를 사용)
```cpp
int num = 2;
int caseValue = 1;
switch (num) {
    case caseValue:  // ❌ 오류! case 뒤에는 정수형 "리터럴"만 가능
        printf("num은 1입니다.\n");
        break;
}
```

#### ? 연산자 (삼항 연산자)

**(conditional_expr) ? expr1 : expr2**

- `conditional_expr`은 boolean 표현식  
  → 표현식이 참이라면 `expr1`의 값을 리턴  
  → 표현식이 거짓이라면 `expr2`의 값을 리턴  
- if-else문의 사용과 유사함

---

## 2. 반복문

반복문이란? **반복 조건 + 명령문**

**사용 예**
- 특정 횟수만큼 반복이 필요할 때
- 집합의 각 요소에 대한 연산이 필요할 때
- 특정 조건이 참일 동안 명령의 수행이 필요할 때
- 입력 스트림의 끝까지 반복을 수행해야 할 때
- 무한 반복이 필요할 때

#### for 문

```cpp
for (초기화; 종료조건; 증감) {
    명령문;
}
```

배열 루프

```cpp
int scores[] = {100, 90, 50};

for (int i = 0; i < 3; i++) {
    std::cout << scores[i] << std::endl;
}
```

콤마 연산자

```cpp
for (int i = 0, j = 5; i < 5; i++, j++) {
    std::cout << i << " * " << " : " << i * j << std::endl;
}
```

for 문에서 모든 조건이 항상 존재할 필요는 없음.

```cpp
int main() {
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
}
```

#### while 문

```cpp
while(expr) {
    statements;
}
```

```cpp
int i = 0;

while (i < 5) {
    std::cout << i << std::endl;
    i++; // important
}
```

Boolean flag 사용 예

```cpp
bool is_done = false;
int number = 0;

while (!is_done) {
    std::cout << "enter number under 10" << std::endl;
    std::cin >> number;
    if (number >= 10) {
        std::cout << "wrong number" << std::endl;
    }
    else {
        std::cout << "OK!" << std::endl;
        is_done = true;
    }
}
```

#### do while 문

```cpp
do {
    statements;
} while (expr);
```

#### continue
- `continue` 문 이후의 문장은 실행되지 않음
- **다음 iteration으로** 곧바로 넘어가기 위해 사용

#### break
- `break` 문 이후의 문장은 실행되지 않음
- **루프 밖으로** 빠져나가기 위해 사용

![cont_break](../images/2025-02-05-Controlling%20Flow/continue_break.png)

---
### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_