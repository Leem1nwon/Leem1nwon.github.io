---
layout: single
title: "[Embedded System] Lab02 : 메모리 정보 분석 실습"
categories: Embedded_System
---

## 1. 메모리 정보 확인

`$ lscpu` 또는 `$ lscpu | grep "cache"

### 1.1. i-cache vs. d-cache

- instruction cache (i-cache, L1i cache)
  - 드문 쓰기 작업
    - 코드가 자주 수정되지 않는다는 가정을 기반으로 설계
    - 코드의 수정은 데이터 수정보다 훨씬 드물기 때문에, 쓰기 작업데 대한 최적화가 덜 필요
    - 전력 소비 최소화 가능
- Data cache (d-cache, L1d cache)
  - 데이터 전달 네트워크
    - 저장-로드 전달 (store to load forwarding)

### 1.2. 파일시스템의 Block size 확인

- 디스크 확인: `$lsblk`
- Block size 확인: `$ tune2fs -l /dev/sdb1`

### 1.3. C의 메모리 모델

```c
int a = 2;  // 전역 변수 (global variable): 컴파일러에 의해 고정된 메모리 위치로 할당
void foo(int b, int* c){
    ...
}

int main(void){
    int d;
    int* e;
    d = ...;                    // Assign some value to d
    e = malloc(sizeInBytes);    // Allocate memory for e
    *e = ...;                   // Assign some value to e
    foo(d, e);                  // heap에 할당되니 포인터 변수 e의 주소값이 stack 변수 c에 저장됨 (pass by reference)
    ...                         // stack에 있는 변수 d의 값이 stack에 할당된 변수 b로 복사됨 (pass by value)
}
```

- pass by value의 경우, d의 값 자체가 복사되어 b로 넘어가므로 b를 수정해도 d의 값은 바뀌지 않음 
- pass by reference 의 경우, 포인터 변수 e가 복사되므로 foo 함수에서 c를 역참조하여 값을 수정할 시 수정이 가능

---

## 2. 메모리 정보 분석

### 2.1. meminfo 정보

- 전문적인 모니터링이나 디버깅을 위해 사용됨
- `$ cat /proc/meminfo`

### 2.2. top 명령어

- 시스템의 전반적인 상태 모니터링
- `$ top`
- `$ htop`: 어디서 병목이 일어나는지 확인

### 2.3. free 명령어

- `free -h`: free 명령어로 현재의 메모리 상태 확인
- Used: 현재 사용 중인 메모리. 특정 프로그램 실행/종료 이후 메모리 증가 여부를 체크하여 메모리 누수 발생 여부를 확인 가능
- Free: 사용 가능한 메모리
- Shared: 공유 메모리
- Buff/cache: 성능 향상을 위해 캐시로 사용하는 메모리