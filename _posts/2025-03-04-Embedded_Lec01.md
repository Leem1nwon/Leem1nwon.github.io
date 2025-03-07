---
layout: single
title: "[Embedded System] Lec01 : Introduction"
categories: Embedded_System
---

# Embedded System
> An embedded system is a device used to control or monitor large-scale systems  
> such as machinery, equipment, or factories.  
> *- The definition of IEEE*

---

## 1. 임베디드 리눅스의 4요소
1. **툴체인 (Toolchain):**  
   타겟 장치를 위한 코드를 만드는데 필요한 컴파일러와 기타 도구로 구성.
2. **부트로더 (Bootloader):**  
   보드를 초기화하고 리눅스 커널을 로드하는 프로그램.
3. **커널 (Kernel):**  
   시스템 자원을 관리하고, 하드웨어와 상호작용.
4. **루트 파일시스템 (Root Filesystem):**  
   커널 초기화 후 실행되는 라이브러리와 프로그램을 포함.

---

## 2. 툴체인

### 2.1 툴체인이란?
- 컴파일러, 링커, 런타임 라이브러리를 포함하는 **컴파일 도구의 집합**.
- 장치에서 실행될 모든 코드를 컴파일하여, 최적의 명령어 세트를 사용해 하드웨어를 효율적으로 활용해야 한다.
- 프로젝트에 필요한 언어 지원과 POSIX 등 시스템 인터페이스의 신뢰할 수 있는 구현이 필수적이다.

### 2.2 툴체인 예시 - C 언어 프로그램의 실행 과정
C 프로그램이 실행 파일로 변환되는 과정은 **전처리(Precompile) → 컴파일(Compile) → 어셈블(Assemble) → 링크(Link)** 순서로 진행된다.

#### 2.2.1 전처리 (Precompile)
- `#` 으로 시작하는 지시문을 처리하는 단계.  
- 예를 들어, `#include` 구문은 다른 파일의 내용을 포함시킨다.

**전처리 전:**
```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, %s\n", name);
}
```

**전처리 후:**
```cpp
string get_string(string prompt);
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, %s\n", name);
}
```

#### 2.2.2 컴파일 (Compile)
- 전처리된 코드를 어셈블리어로 변환하는 단계이며, 이 과정에서 최적화가 수행된다.
  
**컴파일 후 예시:**
```
main:
    .cfi_startproc
    pushq   %rbp
    movq    %rsp, %rbp
    subq    $16, %rsp
    movl    $.L.str, %esi
    callq   get_string
    movq    %rax, -8(%rbp)
    movq    -8(%rbp), %rsi
    callq   printf
```

#### 2.2.3 어셈블 (Assemble)
- 어셈블러가 어셈블리 코드를 **오브젝트 코드**(바이너리 코드)로 변환한다.
  
**어셈블 후 예시:**
```
011111101010101011011011010101010101010101010101...
```

#### 2.2.4 링크 (Link)
- 여러 오브젝트 코드 파일을 **하나의 실행 파일**로 결합하는 단계이다.

**링크 전:** 여러 .o 파일 (예: hello.o, cs50.o, printf.o)  
**링크 후:** 하나의 실행 파일 생성

### 2.3 툴체인 종류

#### 2.3.1 네이티브 (Native)
- 개발 환경(Host)와 실행 환경(Target)이 동일한 시스템.
- 데스크톱/서버에서는 일반적이며, 일부 임베디드 장치에서도 사용된다.

#### 2.3.2 크로스 (Cross)
- 개발 환경과 실행 환경이 서로 다른 시스템.
- 빠른 데스크톱에서 개발 후 임베디드 장치에 코드를 로드하여 테스트.
- 임베디드 장치의 제한된 자원 때문에 크로스 개발 툴체인이 주로 사용된다.

### 2.4 툴체인 - CPU 아키텍처 특징
- CPU 아키텍처 예: ARM, MIPS, x86_64 등.
- 빅엔디안(Big-Endian) vs. 리틀엔디안(Little-Endian)
- 부동소수점 지원 여부.
- ABI: 함수 호출 시 인자 전달 규칙 등.

### 2.5 툴체인 - 도구

| 명령어      | 설명                                                                                           |
|-------------|------------------------------------------------------------------------------------------------|
| addr2line   | 실행 파일 내 디버그 심볼을 사용하여 주소를 파일 이름과 행 번호로 변환                           |
| ar          | 정적 라이브러리를 만들 때 사용                                                                  |
| as          | GNU 어셈블러                                                                                  |
| c++filt     | C++ 및 Java 심볼을 복원                                                                         |
| cpp         | C의 전처리기로, `#include`, `#define` 등의 지시자를 확장                                        |
| elfedit     | ELF 파일의 헤더를 업데이트                                                                      |
| g++         | GNU C++ 프론트엔드                                                                              |
| gcc         | GNU C 프론트엔드                                                                                |
| gcov        | 코드 커버리지 도구                                                                              |
| gdb         | GNU 디버거                                                                                     |
| ld          | GNU 링커                                                                                      |
| nm          | 오브젝트 파일의 심볼을 나열                                                                      |
| objcopy     | 오브젝트 파일 복사 및 변환                                                                      |
| ranlib      | 정적 라이브러리 인덱스 생성하여 링크 단계를 빠르게 함                                             |
| readelf     | ELF 오브젝트 파일의 정보를 출력                                                                 |
| size        | 오브젝트 파일의 섹션 크기 및 전체 크기 출력                                                      |
| strings     | 파일 내 인쇄 가능한 문자열 출력                                                                 |
| strip       | 오브젝트 파일의 디버그 심볼 제거하여 파일 크기 감소 (실행 파일 최적화에 사용)                     |

### 2.6 툴체인 - C 라이브러리

| 라이브러리      | 설명                                                    |
|-----------------|---------------------------------------------------------|
| **libc**        | `printf`, `open`, `close`, `read`, `write` 등 POSIX 함수 포함 |
| **libm**        | `cos`, `exp`, `log` 등 수학 함수 포함                      |
| **libpthread**  | `pthread_`로 시작하는 POSIX 스레드 함수 포함                |
| **librt**       | 공유 메모리와 비동기 I/O 등 POSIX 실시간 확장 포함           |

> ⟶ **libc**는 항상 링크되며, 나머지 라이브러리는 `-l` 옵션으로 명시적으로 링크해야 한다.

### 2.7 툴체인 - 라이브러리 링크

C 프로그램에서 라이브러리 링크 방식에는 **정적 링크 (Static Linking)**와 **동적 링크 (Dynamic Linking)**가 있다.

#### 2.7.1 정적 링크 (Static Linking)
- 라이브러리 함수 코드를 실행 파일에 직접 포함.
- 실행 파일 안에 필요한 코드만 복사되므로 실행 시 별도의 라이브러리가 필요하지 않다.
- 단, 실행 파일 크기가 커질 수 있다.
```bash
gcc -static helloworld.c -o helloworld-static
```

**정적 라이브러리 생성 예:**
```bash
gcc -c test1.c
gcc -c test2.c
ar rc libtest.a test1.o test2.o
```
링크 시:
```bash
gcc helloworld.c -L../libs -I../include -ltest -o helloworld
```

#### 2.7.2 동적 링크 (Dynamic Linking)
- 실행 파일에는 라이브러리 참조 정보만 포함하며, 실행 시 라이브러리가 메모리로 로드된다.
- 실행 파일 크기가 작고, 메모리 사용이 효율적이다.
- 여러 프로그램이 같은 라이브러리를 공유하여 사용하며, 독립적인 업데이트가 가능하다.
```bash
gcc -fPIC -c test1.c
gcc -fPIC -c test2.c
gcc -shared -o libtest.so test1.o test2.o
```
`-fPIC`: 위치 독립 코드 생성
`-shared`: 동적 라이브러리 생성

**링크 시:**
```bash
gcc helloworld.c -L../libs -I../include -ltest -o helloworld
```

#### 2.7.3 공유 라이브러리 버전 번호
- 동적 라이브러리는 버전 관리가 필요하다.
- 주요 파일 유형:
  - `libjpeg.a` → 정적 라이브러리
  - `libjpeg.so` → 동적 라이브러리 심볼릭 링크
  - `libjpeg.so.8` → 실행 시 로드되는 심볼릭 링크
  - `libjpeg.so.8.0.2` → 실제 공유 라이브러리 파일 (버전 8.0.2)

