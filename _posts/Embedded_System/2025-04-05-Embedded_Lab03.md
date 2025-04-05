---
layout: single
title: "[Embedded System] Lab03 : 장치 드라이버 실습"
categories: Embedded_System
---

## 1. Makefile

### 1.1. Makefile 사용하지 않고 빌드할 경우

- 각각의 소스 파일을 개별적으로 컴파일하고 링크해야 함

```bash
# 개별 소스 파일 컴파일
# 링크하지 않고 컴파일만 수행(-c 옵션)
gcc -c -o main.o main.c
gcc -c -o foo.o foo.c
gcc -c -o bar.o bar.c

# 최종 실행 파일 생성을 위한 링크
# 컴파일된 object 파일들을 묶는 링크
gcc -o app.out main.o foo.o bar.o
```

#### 치명적인 실수 가능성
```bash
gcc -o main.c main.c
```
- 이 경우 main.c를 빌드해서 "같은 이름"인 main.c 라는 실행파일로 저장
- 즉 소스파일은 사라지고 빌드된 바이너리만 남게 되는 사고 발생


### 1.2. Makefile을 사용한 빌드 방법

Makefile의 기본 구조는 다음과 같습니다:

```makefile
<Target>: <Dependencies>
    <Recipe>
```

주요 명령어:
- `make`: 전체 실행 파일(app.out) 생성
- `make foo.o`: 특정 target만 빌드

### 1.3. Makefile 주요 변수

Makefile에서 자주 사용되는 변수들:

- `CC`: 컴파일러 지정
- `CFLAGS`: 컴파일 옵션
- `OBJS`: 중간 산출 Object 파일 목록
- `TARGET`: 빌드 대상(실행 파일) 이름

### 1.4. 자동 변수

Makefile에서 사용되는 자동 변수:

- `$@`: 현재 Target 이름
- `$^`: 현재 Target이 의존하는 대상들의 전체 목록
- `$?`: 현재 Target이 의존하는 대상들 중 변경된 것들의 목록

### 1.5. Clean Rule

빌드 중간 산출물 정리를 위한 규칙:

```makefile
clean:
    rm -f *.o
    rm -f $(TARGET)
```

이 규칙은 불필요한 오브젝트 파일과 실행 파일을 제거하는 용도


---
## 2. First Driver

### 2.1. 소스 코드 작성

`driver.c` 

```c
#include <linux/kernel.h>
#include <linux/init.h>
#include <linux/module.h>

// Module init function
static int __init hello_world_init(void)
{
    printk(KERN_INFO "Welcome to Embedded System Class\n");
    printk(KERN_INFO "This is the Simple Module\n");
    printk(KERN_INFO "Kernel Module Inserted Successfully...\n");
    return 0;
}

// Module exit function
static void __exit hello_world_exit(void)
{
    printk(KERN_INFO "Kernel Module Removed Successfully...\n");
}

module_init(hello_world_init);
module_exit(hello_world_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Minwon Lee <minwon0314@hanyang.ac.kr>");
MODULE_DESCRIPTION("A simple hello world driver");
MODULE_VERSION("1.0");
```

### 2.2. Makefile 작성

드라이버 빌드를 위한 Makefile:

```makefile
obj-m += driver.o

KERN_DIR ?= /lib/modules/$(shell uname -r)/build

all:
    make -C $(KERN_DIR) M=$(shell pwd) modules

clean:
    make -C $(KERN_DIR) M=$(shell pwd) clean
```

### 2.3. 빌드 및 실행

1. **빌드 과정**
    ```bash
    make all
    ```

2. **커널 로드 확인**
    ```bash
    sudo insmod driver.ko  # 커널에 모듈을 올리는 명령어 (insert module)
    lsmod | grep driver    # 어떤 모듈이 올라와 있는지 확인
    ```
    실행 결과로 driver 모듈이 커널에 로드된 것을 확인 가능

3. **출력 메시지 확인**
    ```bash
    sudo dmesg  # printk로 작성했던 내용들을 확인할 수 있음
    ```
다음과 같은 메시지가 출력: 

```bash
Welcome to Embedded System Class
This is the Simple Module
Kernel Module Inserted Successfully...
```

### 2.4. 드라이버 정보 확인

`modinfo` 명령어를 통해 드라이버 정보를 확인 가능:
```bash
modinfo driver.ko
```

주요 정보:
- description: A simple hello world driver
- author: Soowon Hwang
- version: 1.0
- license: GPL

