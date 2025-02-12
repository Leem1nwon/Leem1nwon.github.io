---
layout: single
title: "Inheritance"
---

# 상속

## 1. 상속이 필요한 이유

#### 1-1. 코드 예시

**Player 클래스**
```cpp
class Player
{
private:
    int x, y;   // 플레이어의 현재 위치 좌표
    int speed;  // 플레이어의 이동 속도
public:
    Player(int x, int y, int speed)  // 생성자: 초기 위치(x, y)와 이동 속도(speed)를 설정
        : x{ x }, y{ y }, speed{ speed }
    {}

    void Move(int dx, int dy)  // 이동 함수: 입력된 방향(dx, dy)에 speed를 곱하여 위치 변경
    {
        x += dx * speed;
        y += dy * speed;
    }

    void ShowPosition()  // 현재 위치 출력 함수
    {
        cout << x << "," << y << endl;
    }
};
```

**Player 관리를 위한 클래스(컨트롤/매니저 클래스)**
```cpp
class PlayerHandler
{
private:
    Player* playerList[50];  // 최대 50명의 Player 객체를 관리하는 배열 (Player 객체의 포인터 저장)
    int playerNum;  // 현재 등록된 플레이어 수
public:
    // 생성자: 초기 등록된 플레이어 수를 0으로 설정
    PlayerHandler() : playerNum{ 0 } {}

    // Player 객체의 포인터를 배열에 추가하고, 등록된 플레이어 수를 증가시킴
    void AddPlayer(Player* p)
    {
        playerList[playerNum++] = p;  
    }

    // 모든 플레이어의 현재 위치를 출력하는 함수
    void ShowAllPlayerPosition() const
    {
        for (int i = 0; i < playerNum; i++)
        {
            playerList[i]->ShowPosition();  
            // playerList[i]는 Player 객체를 가리키는 포인터이므로,
            // -> 연산자를 사용하여 Player 객체의 멤버 함수 ShowPosition() 호출
        }
    }

    // 소멸자: 등록된 모든 Player 객체를 삭제하여 동적 할당된 메모리 해제
    ~PlayerHandler()
    {
        for (int i = 0; i < playerNum; i++)
        {
            delete playerList[i];  // 동적으로 할당된 Player 객체 삭제
        }
    }
};
```

**main 함수**
```cpp
int main()
{
    PlayerHandler playerHandler;
    playerHandler.AddPlayer(new Player(1, 1, 1));
    playerHandler.AddPlayer(new Player(5, 5, 1));
    playerHandler.AddPlayer(new Player(2, 3, 1));
    playerHandler.ShowAllPlayerPosition();
}
```

#### 1-2. 상속이 필요한 이유
- 만약 Plyaer 이외에 Enemy, NPC가 추가된다면?
- Enemy와 NPC의 이동 방식이 다르다면?
  - Enemy : dx * speed * 1.5f;
  - NPC : 이동 불가능
- Enemy 및 NPC 클래스를 추가 구현했을 때, 컨트롤 클래스를 얼마나 수정해야 하는가?
  - 멤버 변수 추가 필요 (ex. enemyNum, npcNum)
  - 클래스별 정보 추가 기능 필요
  - 클래스별 위치 정보 출력 반복문 필요
  - 클래스별 해제 필요 등..

⟶ **상속과 다형성을 활용한다면, 적은 수정으로 기능 추가가 가능하도록 설계할 수 있다!** 

---

## 2. 상속의 정의
- 상속이란, 기존 클래스를 활용하여 세로운 클래스를 생성하는 방법이다.
- 새로운 클래스는 **기존 클래스의 데이터와 행동(함수)를 포함**
- 기존 클래스를 **재사용** 가능하게 함
- 클래스들 간의 공통 속성에 집중하는 설계 방법
- 기존 클래스의 행동(함수)을 수정하여, 새로운 클래스의 행동을 새로 정의 가능
  - 기존 클래스의 행동을 수정할 필요는 없음

**상속을 사용하지 않은 예**
```cpp
class Player {
    // x, y, speed, hp, xp, ...
};

class Enemy {
    // x, y, speed, xp, gold...
};

class NonPlayer {
    // x, y, dialog, ...
};
```

**상속을 사용한 경우**

```cpp
class Entity {
    // x, y, Talk() ⟶ 공통 데이터, 공통 기능 
};

class Player : public Entity {
    // speed, hp, xp, Move()
};

class Enemy : public Entity {
    // speed, hp, gold, Move()
};

class NonPlayer : public Entity {
    // dialog
};
```
**Player 예시 설계에서 상속을 사용 시**
- 코드의 재사용 가능
- Entity의 모든 데이터와 기능의 Entity를 상속한 클래스들에 포함됨
- Player 객체는 **두 가지 타입을 모두 가진 상태**가 됨
  - Player 객체는 Player 타입이기도 하지만, Entity 타입이기도 함!


**기본 클래스 (base class/parent class/super class)**
- 상속의 대상이 되는 클래스 

**유도 클래스 (derived class/child class/sub class)**
- 기본 클래스로부터 생성되는 클래스
- 데이터와 행동을 기본 클래스로부터 상속함

---

## 3. 유도 클래스

#### 3-1. C++ 상속 문법

```cpp
class Base{
    // base class members...
};

class Derived : public Base{
    // Derived class members...
};
```

- public 상속
  - 가장 흔히 사용되는 상속 방식
  - "is-a" 관계의 정의와 가장 일치하는 상속 방식


```cpp
#include <iostream>
using namespace std;

class Entity
{
protected:
	int x;
	int y;
public:
	Entity(int x, int y)	// 생성자
		: x{ x }, y{ y }{}
	void ShowPosition()
	{
		cout << "[" << x << "," << y << "]" << endl;
	}
	void Talk()
	{
		cout << "Hello." << endl;
	}
};

class Player : public Entity
{
private:
	int hp;
	int xp;
	int speed;
public:
	Player(int x, int y, int speed)
		: Entity{ x, y }, speed{ speed }{}
	void Move(int dx, int dy)
	{
		x += dx; y += dy;	// x, y 가 Entity의 protected 멤버 변수이므로, 유도 클래스의 멤버 함수가 접근 가능
	}						// x, y 가 만약 private 멤버 변수였다면, 유도 클래스의 멤버 함수가 접근할 수 없음
	void SetHP(int hp)
	{
		if (hp < 0)
		{
			return;
		}
		this->hp = hp;
	}
};

int main()
{
	Entity e{ 1, 1 };
	e.ShowPosition();
	e.Talk();

	Player p{ 5, 5, 10 };
	p.Move(1, -1);
	p.ShowPosition();
	p.Talk();
	// p.hp = 10;	ERROR! private 멤버 변수 접근 불가
	p.SetHP(10);	// OK
}
```

---

## 4. Protected Member


```cpp
class Base
{
protected:
    // protected members
};
```

- 기본 클래스에서 접근 가능
- 유도 클래스에서 접근 가능
- 기본 또는 유도 클래스의 객체로부터는 접근 불가능!
  - private의 경우, 클래스에서는 접근 가능하지만 객체로부터의 접근은 불가능했음
  - protected는 **상속이 이루어지는 private**
  - private는 유도 클래스에서 접근이 불가능함에 유의
    - private는 상속과 관계없이 무조건 (자신) 클래스 내부에서만 접근 가능함


```cpp
class Base
{
public:
    int a;

protected:
    int b;

private:
    int c;
};

class Derived: public Base
{
public:
    void SetValue()
    {
        cout << a << endl;  // OK!
        cout << b << endl;  // OK!
        cout << c << endl;  // ERROR!
    }
};

int main()
{
    Derived d;
    d.a = 1;    // OK
    d.b = 2;    // ERROR!
    d.c = 3;    // ERROR!

    cout << sizeof(Base) << endl;       // 12 bytes
    cout << sizeof(Derived) << endl;    // 12 bytes
}
```
- 접근할 수 없는 것이지, 값이 메모리에 저장되지 않는 것은 아님 
---

## 5. 상속에서의 생성자와 소멸자

#### 5-1. 상속에서의 생성자
- 유도 클래스는 기본 클래스의 멤버를 포함하므로, 유도 클래스가 초기화 되기 **이전에** 기본 클래스에서 상속된 부분이 반드시 초기화 되어야 함
- 유도 클래스 객체가 생성될 때,
  - 먼저 기본 클래스의 생성자가 호출되고,
  - 그 이후 유도 클래스의 생성자가 호출됨

![constructor](../images/2025-02-10-Inheritance/constructor.png)
![destructor](../images/2025-02-10-Inheritance/destructor.png)

#### 5-2. 상속에서의 소멸자
- 소멸자는 생성자와 반대 순서로 호출됨
- 즉, 유도 클래스가 소멸될 때,
  - 먼저 유도 클래스의 소멸자가 호출되고
  - 그 이후 기본 클래스이 소멸자가 호출됨

## 6. 기본 클래스 생성자와의 관계 

#### 6-1. 생성자 오버로딩딩

```cpp
#include <iostream>
using namespace std;

class Base
{
private:
	int value;
public:
	Base() : value{ 0 } { cout << "Base no-args constructuor" << endl; }
	Base(int x) : value{ x } { cout << "Base (int) overloaded constructuor" << endl; }
	~Base() { cout << "Base destructor" << endl; }
};

class Derived : public Base
{
private:
	int doubled_value;
public:
	Derived() : doubled_value{ 0 } { cout << "Derived no args constructor" << endl; }
	Derived(int x) : doubled_value {x*2} { cout << "Derived (int) overloaded constructor" << endl; }
	~Derived() { cout << "Derived destructor" << endl; }
};

int main()
{
	Base b1;	// value: 0
	Base b2{ 100 };	// value: 100
	Derived d1;	// value: 0, doubled_value: 0
	Derived d2{ 1000 };	// value: 0, doubled_value: 2000
}
```
- 기본 클래스의 생성자로 인자 전달
  - 기본 클래스의 어떤 생성자를 호출할지 결정해 줄 수 있어야 함
  - **유도 클래스의 생성자에, 초기화 리스트를 활용해 사용자가 원하는 기본 클래스의 생성자 호출 가능**


```cpp
class Base {
public:
    Base();
    Base(int);
};

Derived::Derived(int x) // 유도 클래스 생성자
    : Base{x}, {...}    // 초기화 리스트 자리에 사용자가 원하는 기본 클래스의 생성자 호출 가능
{
    // derived constructor code
}
```

```cpp
class Base {
private:
    int value;
public:
    Base() : value{0} { cout << "Base no-args constructor" << endl; }
    Base(int x) : value{x} { cout << "Base (int) overload constructor" << endl; }
};

class Derived : public Base {
private:
    int doubled_value;
public:
	Derived() : Base{}, doubled_value{ 0 } { cout << "Derived no args constructor" << endl; }
	Derived(int x) : Base{x}, doubled_value {x*2} { cout << "Derived (int) overloaded constructor" << endl; }
};

int main()
{
	Base b1;	// value: 0
	Base b2{ 100 };	// value: 100
	Derived d1;	// value: 0, doubled_value: 0
	Derived d2{ 1000 };	// value: 1000, doubled_value: 2000
}
```

#### 6-2. 복사 생성자와 상속
- 컴파일러가 자동생성 하지만, 필요한 경우 직접 구현 해야함
- 기본 클래스에서 구현한 복사 생성자 호출 가능

**유도 클래스의 복사 생성자**
- 기본 클래스의 복사 생성자를 직접 호출 가능
- slice 과정을 거침


```cpp
Derived::Derived(const Derived &other)
    : Base{other}, {Derived initialization list}
{
    //code
}

/* other는 유도 클래스이지만, slice를 통해 기본 클래스의 복사 생성자에 
인수로 넘겨줄 수 있다. 유도 클래스 is a 기본 클래스 관계를 준수하기 때문! */
```


```cpp
class Base{
    int value;
public:
    ...
    Base(const Base &other) : value{other.value}
    { cout << "Base Copy constructor" << endl; }
};

class Derived : public Base {
    int doubled_value;
public:
    ...
    Derived(const Derived &other)
        : Base{other}, doubled_value{other.doubled_value} { // ✅ Base의 복사 생성자 명시적 호출
            cout << "Derived Copy Constructor" << endl;
        }
};
```

**복사 생성자의 구현 가이드**
- 유도 클래스에서 사용자가 복사 생성자를 구현하지 않은 경우,
  - 컴파일러가 자동으로 생성하며, 기본 클래스의 복사 생성자를 호출
- 유도 클래스에서 사용자가 복사 생성자를 구현한 경우,
  - 명시하지 않으면 **기본 클래스의 기본 생성자**(인자를 받지 않는 생성자)를 호출
  - **기본 클래스를 위한 복사/이동 생성자를 명시적으로 호출해 주어야 함**
- 따라서, 포인터형 멤버 변수를 가지고 있는 경우, 기본 클래스의 복사/이동 생성자를 호출하는 방법에 대해서 반드시 숙지해 두어야 함.

---

## 7. 상속과 멤버 함수

#### 7-1. 기본 클래스의 멤버 함수 사용
- 유도 클래스는 기본 클래스의 멤버 함수를 직접 호출 가능
  - public / protected인 멤버 함수의 경우에
- 유도 클래스는 기본 클래스의 멤버 함수를 **오버라이드 또는 재정의 가능**
- 다형성의 구현을 위해 중요한 기능
- 재정의로 인해 기본 클래스 함수가 가려질 경우,
  - Base::Member()와 같은 형식으로 호출 가능

 #### 7-2. 예시 코드

 ```cpp
 class Entity {
protected:
    int x, y;
public:
    Entity(int x, int y)
        : x{ x }, y{ y }
    {}
    void Talk() {
        cout << "Hello";
    }
    void ShowPosition() {
        cout << x << "," << y << endl;
    }
};

class Player : public Entity {
private:
    int speed;
    int hp;
    int xp;
public:
    Player(int x, int y, int speed)
        : Entity(x, y), speed{ speed }
    {}
    void Talk() {   // 기본 클래스의 Talk 함수를 유도 클래스에서 재정의 
        cout << "Hello. I'm Player";
    }
};
 ```

#### 7-3. 정적 바인딩
- 유도 클래스와, 기본 클래스에 같은 이름과 인자를 갖는 함수가 2개 존재 (Talk())
- 어떤 기준으로 호출할 함수를 결정할까? ⟶ 타입을 기준으로 호출!
- 정적 바인딩은, 컴파일 시 어떤 함수가 호출될지를 결정하는 방식
- C++ 의 기본은 정적 바인딩
  - 가장 아래의 경우, 동적 바인딩을 사용하여 기능의 확장 가능


```cpp
Entity e{1,1};
e.Talk();   // Entity::Talk call

Player p{1,1,2};
p.Talk();   // Player::Talk call

Entity *ePtr = new Player{1,1,2};
ePtr->Talk()    // Entity::Talk call
```

#### 7-4. Is-A 관계
- 기본 클래스 객체를 사용하는 곳에는 항상 유도 클래스 객체를 사용 가능
  - 유도 클래스에 기본 클래스의 멤버가 모두 포함되기 때문 (superset)
  - player 객체는 두 가지 타입을 모두 가진 상태가 됨!

```cpp
void TalkSomething(Entity e)    // Entity 타입 객체를 매개변수로 받음  
{
    e.Talk();
}

void ShowSomething(Entity e)
{
    e.ShowPosition();
}

int main()
{
    Player p{0,0,1};
    TalkSomething(p);   // Player 객체 p를 인자로 넘겨도 오류 발생하지 않음
    ShowSomething(p);
}
```

```cpp
int main()
{
    Entity p = Player{1,1,1};  // 객체 slicing 발생
    Entity* ePtr = new Player{1,1,1};  // 업캐스팅 허용
    Entity& pRef = p;  // 참조 변수는 기본 클래스 타입으로 캐스팅 가능
    pRef.ShowPosition();

    Player* pPtr = new Entity{1,1,1};  // ⚠️ 오류 발생!
}
```

---
### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했음.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_

