---
layout: single
title: "[Embedded System] Lec03 : 장치 드라이버"
categories: Embedded_System
---

## 1. 리눅스 

### 1.1. 리눅스 커널이란?
- 운영체제의 핵심 프로그램
- 컴퓨터의 물리적/추상화 자원(디바이스, 프로세스, 메모리 등)을 관리

### 1.2. 리눅스 아키텍처
- 커널 공간: **운영체제를 실행**시키기 위해 필요한 **메모리 공간**
- 사용자 공간: **프로그램이 동작**하기 위해 사용되는 **메모리 공간**
  - **시스템 호출 인터페이스**를 통해 상호작용
  - 커널이 다루는 중요한 자원에 일반 사용자가 접근하기 못하도록 하기 위함

---
  
## 2. 장치 드라이버

### 2.1. 장치
- 네트워크 어댑터, 디스크, USB, 프린터, LCD 디스플레이, Audio, 키보드 등과 같은 주변 장치들을 말함

### 2.2. 장치 드라이버
- 실제 **장치 부분을 추상화**시켜 사용자 프로그램이 **정형화된 인터페이스**를 통해 디바이스에 접근할 수 있도록 해주는 프로그램 
- 디바이스 관리에 필요한 정형화된 인터페이스 구현에 요구되는 **함수와 자료구조의 집합체**
- 표준적으로 동일 서비스 제공을 목적으로 커널의 일부분으로 내장
- 응용프로그램이 HW를 제어할 수 있도록 인터페이스 제공
- HW 독립적인 프로그램 작성을 가능하게 함

### 2.3. 장치 드라이버 인터페이스
- Standard Device Driver Interface
  - UNIX compatible I/O system interface
    : open(), close(), read(), write(), ioctl()

### 2.4. 사용자 관점에서의 리눅스 장치 드라이버
- 사용자는 디바이스 자체에 대한 자세한 정보를 알 필요 없음
  - 사용자가 모든 HW 규격에 맞춰서 개발을 하는 것은 매우 비효율적
  - 장치 부분의 **추상화** 
- **Device는 하나의 파일로 인식됨**
- 사용자는 **File에 대한 접근을 통하여** Real Device에 접근 가능

| 계층 | 구성 요소 |
|:---:|:---:|
| 상위 계층 | User Program |
| ⬇️ | |
| | Device file |
| ⬇️ | |
| | VFS (Virtual File System) |
| ⬇️ | |
| | Device Driver |
| ⬇️ | |
| 하위 계층 | Real Device |

### 2.5. 리눅스 장치 드라이버의 필요성

#### 문제점
- 일반적인 컴퓨팅 시스템(노트북, 데스크탑, 휴대폰)은 다양한 장치들(키보드, 마우스, LCD)을 수반함
- 하드웨어 장치마다 제어 방법이 다르기 때문에, 이를 직접 제어하려면 복잡하고 번거로움
- 매번 장치 프로그램에 장치 제어 코드를 넣으면, 유지보수도 어렵고 장치가 바뀔 때마다 수정 필요

#### 운영체제의 역할
- 운영체제는 각 장치마다 장치 드라이버를 제공
- 애플리케이션이 HW를 직접 다루는 게 아니라, **운영체제를 통해 간접적**으로 다루는 것
- 이를 **운영체제가 장치 드라이버를 호스팅**한다고 표현함 
- 또한 모든 장치에 대해 **단일한 인터페이스를 제공**함
    ⟶ 어떤 장치를 사용해도 운영체제가 해당 장치에 맞는 인터페이스를 알아서 제공

#### 보안 측면
- 장치 드라이버는 운영체제에 의해 관리되기 때문에, **악의적인 접근이나 잘못된 사용으로부터 보호** 가능
- 따라서 운영체제는 **필요한 보안을 제공**

---

## 3. 리눅스 커널 모듈

### 3.1. 리눅스 커널 모듈이란?
- 요구에 따라 커널에 로드/언로드 될 수 있는 코드 조각들
- 리눅스 커널에 커스텀 코드를 추가하는 방법
    1. 코드를 커널 소스 트리에 추가하고 재컴파일 (번거로움)
    2. 커널이 실행 중일 때 코드를 추가 ⟶ **로드 가능 커널 모듈**

### 3.2. 로드 가능 커널 모듈 사용 예
- **A. 리눅스 장치 드라이버**
- B. 파일 시스템 드라이버
- C. 시스템 콜

### 3.3. 로드 가능 커널 모듈의 장점
- 새로운 장치가 추가될 때마다 **커널을 다시 빌드할 필요가 없음**
- 유연성: 한 줄의 명령으로 로드/언로드 될 수 있음
    ⟶ 필요할 때만 LKM을 로드할 수 있기 때문에 메모리를 절약할 수 있음

### 3.4. 커널 모듈과 사용자 프로그램과의 차이
- 커널 모듈은 별도의 주소 공간을 가짐
  - 커널 모듈은 커널 공간에 존재
  - 사용자 프로그램(애플리케이션)은 사용자 공간에 존재
    ⟶ 시스템 소프트웨어를 사용자 프로그램으로부터 보호
- 더 높은 실행 권한을 가짐
- 순차적으로 실행되지 않음
- 다른 헤더 파일을 사용

### 3.5. 커널 드라이버 vs. 커널 모듈
- 커널 모듈은 런타임에 커널에 삽입될 수 있는 컴파일된 코드 조각
  - 예) insmod, modprobe 명령 이용
- 커널 드라이버는 어떤 하드웨어 장치와 통신하는 코드 조각
  - 하드웨어를 구동
  - 거의 모든 하드웨어에는 관련 드라이버가 있음

---

## 4. 리눅스 장치 드라이버 종류

### 4.1. 문자 장치
- 데이터를 한 글자씩 읽고/쓰는 하드웨어 파일 (키보드, 마우스 등)
- 사용자가 문자 파일을 사용하여 데이터를 작성하는 경우, 다른 사용자는 그 문자 파일을 사용할 수 없음
- 통신 목적으로 사용됨

### 4.2. 블록 장치
- 데이터를 블록 단위로 읽고/쓰는 하드웨어 파일
- 대량의 데이터를 읽고/쓰고 싶을 때 유용 (HDD, USB)

### 4.3. 장치 드라이버 정리

| Device Driver 종류 | 설명 | 등록 함수명 |
|:---:|:---|:---:|
| 문자 드라이버 (char) | • device를 파일처럼 접근하여 직접 read/write 수행<br>• data 형태는 **stream** 방식으로 전송<br>e.g.) console, keyboard, serial port driver 등 | register_chrdev() |
| 블록 드라이버 (block) | • disk와 같은 file system을 기반으로 **block 단위**로 데이터를 read/write<br>e.g.) hard disk, CD-ROM driver, floppy disk | register_blkdev() |

### 4.4. 문자 장치 추가 설명
- 자료의 순차성을 지닌 장치
- 버퍼 캐쉬를 사용하지 않음
- 장치의 Raw data를 사용자에게 제공
- Terminal, Serial/Parallel, Keyboard, Printer

### 4.5. 블록 장치 추가 설명
- Random Access 가능
- 블록 단위의 입출력이 가능한 장치
- 버퍼캐쉬에 의한 내부 장치 표현
- 파일 시스템에 의해 mount되어 관리되는 장치
- 디스크, RAM disk, CD-ROM 등

---

## 5. 커널 모듈 프로그래밍

### 5.1. 간단한 Hello World 드라이버 예제

```c
/********************************************************
 * \file driver.c
 *
 * \details Simple hello world driver
 *
 * \author EmbeTronicX
 *
 ********************************************************/
/* 헤더 파일 추가 */
#include<linux/kernel.h>
#include<linux/init.h>
#include<linux/module.h>
/*
** Module Init function
*/
static int __init hello_world_init(void)    /* 장치 드라이버 켤 때 */
{
    printk(KERN_INFO "Welcome to EmbeTronicX\n");
    printk(KERN_INFO "This is the Simple Module\n");
    printk(KERN_INFO "Kernel Module Inserted Successfully...\n");
    return 0;
}
/*
** Module Exit function
*/
static void __exit hello_world_exit(void)    /* 장치 드라이버 끌 때 */
{
    printk(KERN_INFO "Kernel Module Removed Successfully...\n");
}
module_init(hello_world_init);
module_exit(hello_world_exit);
MODULE_LICENSE("GPL");
MODULE_AUTHOR("EmbeTronicX <embetronicx@gmail.com>");
MODULE_DESCRIPTION("A simple hello world driver");
MODULE_VERSION("2:1.0");
```

### 5.2. 모듈 정보
- `MODULE_LICENSE()`: 모듈의 라이선스를 명시
- `MODULE_AUTHOR()`: 모듈 작성자 정보
- `MODULE_DESCRIPTION()`: 모듈에 대한 설명
- `MODULE_VERSION()`: 모듈의 버전 정보

### 5.3. 모듈의 시작/종료
- 사용자 프로그램과는 다른 종류의 헤더 파일을 필요로 함
- 사용자 공간의 라이브러리/API/시스템콜을 호출할 수 없음
#### 5.3.1. `Init function`
- insmod 명령어를 사용하여 드라이버를 로드하면 실행
- `module_init()`: 매크로를 이용하여 등록

#### 5.3.2. `Exit function`
- 드라이버가 커널에서 언로드될 때 마지막으로 실행되는 함수
- rmmod 명령어를 사용하여 드라이버를 언로드하면 실행
- `module_exit()` 매크로를 이용하여 등록하여야 함

### 5.4. printk() 함수
- 커널 공간에서 사용 가능한 값/메세지 출력을 위한 함수
- 로그 레벨과 우선순위를 연결하여 메세지를 심각성에 따라 분류 가능
  - `KERN_EMERG`: 크래시를 앞두고 발생하는 것과 같이, 긴급 메시지에 사용
  - `KERN_ALERT`: 즉각적인 행동을 요구하는 상황
  - `KERN_CRIT`: 심각한 HW/SW 실패와 관련된 중대한 조건
  - `KERN_ERR`: 오류 조건을 보고하는데 사용 (HW 문제를 보고하기 위해여 사용)
  - `KERN_WARNING`: 시스템에 심각한 문제를 직접적으로 유발하지는 않지만, 문제가 될 수 있는 상황
  - `KERN_NOTICE`: 정상적인 상황이지만 주목할 만한 경우 (보안관련 조건)
  - `KERN_INFO`: 정보 메시지 (시작 시 찾은 HW에 대한 정보 출력)
  - `KERN_DEBUG`: 디버깅 메시지에 사용

### 5.5. printk() vs. printf()
- printk()
  - 다른 **로그 레벨에 따라** 출력이 가능한 **"커널 수준"** 함수
  - dmesg 명령어를 사용하여 출력물들을 확인
- printf()
  - 사용자 프로그램에서 흔히 사용하는 C언어 함수로서 **"사용자 공간"** 함수
  - 항상 파일 디스크립터에 출력
  - STD_OUT 콘솔에서 출력물들을 확인

---
## 6. 하드웨어 장치와의 통신

### 6.1. 통신의 전체 경로
1. 애플리케이션이 장치 파일을 오픈 ⟶ 장치 파일 생성
2. 장치 파일이 대응되는 장치 드라이버를 찾음
    - 주 번호와 부 번호 활용
3. 장치 드라이버가 하드웨어 장치와 통신

```c
User Program
     ↓
Device file
     ↓ (major number, minor number)
Device Driver
     ↓
Real Device
```

### 6.2. 리눅스 커널의 기본 특징: 장치 처리의 추상화
- 모든 하드웨어 장치에 대해 특수 파일을 생성
- 파일 조작에 사용되는 표준 시스템 호출을 사용

    **⟶ 즉, 리눅스는 모든 장치를 "파일"처럼 추상화해서, 파일 다루는 방식 그대로 장치도 다룰 수 있도록 한다!**

### 6.3. 주 번호와 부 번호

#### 주 번호
- 어떤 장치 드라이버와 연결되는지 식별하는 번호
- 예를 들어, 4번 주 번호는 tty 계열 장치 드라이버에 연결됨
- 여러 장치에 의해 공유될 수 있음

#### 부 번호
- 같은 드라이버를 쓰는 장치들끼리 구분하기 위한 번호
- 예: tty0, tty1, tty2 …
- 이들은 모두 같은 드라이버(주 번호)를 쓰지만, 부 번호로 개별 장치임을 나타냄.

### 6.4. 주 번호와 부 번호 할당

#### 1. 정적 할당 (Statically allocating)
- 드라이버에 특정한 주 번호 설정 (사용 가능한 경우 해당 주 번호를 할당)
- `int register_chrdev_region(dev_t first, unsigned int count, char *name);`
  - `first`: 할당하려는 범위의 시작 장치 번호
  - `count`: 요청하는 연속적인 장치 번호의 총 수
  - `name`: 이 번호 범위와 관련되어야 하는 장치의 이름 (/proc/devices와 sysfs에 나타남)
  - 반환값: 할당이 성공적으로 수행된 경우 0
- 주 번호에 12bits, 부 번호에 20bits가 할당된 32bit 값
- 예시:
```c
MKDEV(int major, int minor);  // dev_t 구조체 변수 생성
MAJOR(dev_t dev);            // 주/부 번호 반환
MINOR(dev_t dev);
```
```c
dev_t dev = MKDEV(235, 0);
register_chrdev_region(dev, 1, "ECL3003_dev");  // 예시
```

#### 2. 동적 할당 (Dynamically allocating)
- 고정된 주/부 번호를 원하지 않는 경우 (사용 가능한 경우 동적으로 주 번호를 할당)
- `int alloc_chrdev_region(dev_t *dev, unsigned int firstminor, unsigned int count, char *name);`
  - `dev`: 동적 할당 성공시 할당된 범위의 첫 번호를 보유하게 될 출력 전용 매개변수
  - `firstminor`: 사용하고자하는 첫 번째 부 번호 (보통 0)
  - `count`: 요청하는 연속적인 장치 번호의 총 수
  - `name`: 번호 범위와 관련되어야 하는 장치의 이름

### 6.5. 주 번호와 부 번호 할당/해제

#### 정적 할당과 동적 할당의 차이 
- 정적 할당
  - 사전에 시작하고 싶은 주 번호가 있는 경우에 유용
- 동적 할당
  - (Pros) **다른 장치 드라이버와의 충돌을 피하기 위해** 동적 할당이 선호될 수 있음
  - (Cons) **모듈에 할당된 주 번호가 변할 수 있기 때문에**, 사전에 장치 노드를 생성할 수 없음

#### 장치 번호 해제
- 장치를 더 이상 사용하지 않을 때, 정적/동적 할당에 관계없이 해제 필요
`void unregister_chrdev_region(dev_t first, unsigned int count)`

### 6.6. 장치 파일
- 사용자 공간 애플리케이션과 하드웨어 간의 통신을 가능하도록 함
- **일반적인 '파일'이 아니지만, 사용자 공간 애플리케이션을 파일처럼 접근 가능**
- 사용자 애플리케이션이 장치 파일에 접근
  - 커널은 I/O 요청을 인식하고 장치 드라이버에게 전달
  - 드라이버는 직렬 포트에서 데이터를 읽거나 하드웨어에 데이터를 보내는 등의 작업 수행

⟶ **응용 프로그램 프로그래머가 기본 장치의 작동 방식을 알 필요 없이 시스템 리소스에 접근할 수 있는 편리한 방식 제공**

- 모든 장치 파일들은 /dev 디렉토리에 저장됨
  - 모든 장치 파일이 실제 하드웨어에 대응되는 것이 아님

### 6.7. 수동으로 장치 파일 생성

#### 1. 수동 장치 파일 생성 방법
- Makefile을 활용하여 드라이버 빌드
- 드라이버 로드: `insmod`
- 장치 파일 생성: `mknod`

#### 2. mknod 명령어
```bash
mknod -m <permissions> <name> <device type> <major> <minor>
```
- `-m <permissions>`: 새 장치 파일의 퍼미션 설정
- `<name>`: 전체 경로를 가진 장치 파일 이름 (예: /dev/name)
- `<device type>`: c 또는 b
  - c: 문자 장치
  - b: 블록 장치
- `<major>`: 드라이버의 주 번호
- `<minor>`: 드라이버의 부 번호

예시:
```bash
sudo mknod -m 666 /dev/etx_device c 246 0
```
