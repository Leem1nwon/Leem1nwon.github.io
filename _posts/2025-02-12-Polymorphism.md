---
layout: single
title: "Polymorphism"
---

# 다형성

## 1. 다형성과 동적 바인딩

#### 1.1 정적 바인딩
- 정적 바인딩(Compile-time): 함수 오버로딩, 연산자 오버로딩 등에서 사용됨.
- 동적 바인딩(Run-time): 가상 함수 등을 통해 런타임에 결정됨.

**정적 바인딩의 예시 1**
```cpp
Entity entity{0,0};
entity.Move(1,1);   // Entity::Move() 호출

Player player{0,0,2};
player.Move(1,1);   // Player::Move() 호출

Enemy enemy{0,0,2};
enemy.Move(1,1);    // Enemy::Move() 호출

Boss boss{0,0,2};
boss.Move(1,1);     // Boss::Move() 호출

Entity *ePtr = new Boss{0,0,2};
ePtr->Move(1,1);    // Entity* 타입 기준 -> Entity::Move() 호출

/* 컴파일러는 ePtr의 선언 타입(Entity*)를 기준으로 호출할 함수를 미리 결정함 (정적 바인딩) */
```

**정적 바인딩의 예시 2**
```cpp
void DisplayPosition(const Entity& e)
{
    e.ShowPosition();
    // Entity::ShowPosition() 호출
}

Entity entity{0,0};
DisplayPosition(entity);

Player player{0,0,2};
DisplayPosition(player);

Enemy enemy{0,0,2};
DisplayPosition(enemy);

/* 컴파일러는 e의 선언 타입(Entity&)을 기준으로 호출할 함수를 결정함 */
```

#### 1.2 런타임 다형성
- 런타임 다형성은 같은 함수에 대해 실제 객체의 타입에 따라 다른 동작을 하게 한다. (함수 오버라이딩)
- C++에서 이를 구현하기 위해 필요한 조건은:
  - **상속**
  - **기본 클래스 포인터 또는 참조자**
  - **가상 함수**

#### 1.3 동적 바인딩

**동적 바인딩의 예시 1**
```cpp
Entity entity{0,0};
entity.Move(1,1);   // Entity::Move()

Player player{0,0,2};
player.Move(1,1);   // Player::Move()

Enemy enemy{0,0,2};
enemy.Move(1,1);    // Enemy::Move()

Boss boss{0,0,2};
boss.Move(1,1);     // Boss::Move()

Entity *ePtr = new Boss{0,0,2};
ePtr->Move(1,1);    // Boss::Move() !!!!! 

/* 만약 Entity::Move()가 virtual로 선언되었다면, ePtr->Move(1,1)은 Boss::Move()를 호출한다. */
```

**동적 바인딩의 예시 2**
```cpp
void DisplayPosition(const Entity& e)
{
    e.ShowPosition();
    // 만약 ShowPosition()이 virtual로 선언되었다면, 실제 객체에 해당하는 함수가 호출됨.
}

Entity entity{0,0};
DisplayPosition(entity);

Player player{0,0,2};
DisplayPosition(player);

Enemy enemy{0,0,2};
DisplayPosition(enemy);
```

#### 1.4 정리
- **정적 바인딩**: 컴파일 시 선언된 타입을 기준으로 호출할 함수를 결정.
- **동적 바인딩**: 런타임 시 실제 객체의 타입을 기준으로 호출할 함수를 결정.

## 2. 가상함수

#### 2.1 가상함수의 개념 
- 기본 클래스의 함수를 유도 클래스에서 재정의(오버라이드)하여 사용할 수 있도록 하는 함수.
- 예를 들어, Entity::Move()가 가상 함수로 선언되면, 유도 클래스에서 오버라이드한 Move()가 동적 바인딩되어 호출된다.

#### 2.2 가상함수의 선언
```cpp
class Player : public Entity {
public:
    virtual void Move(int dx, int dy);
    // ...
};
```
- 유도 클래스에서는 기본 클래스의 가상 함수와 동일한 원형과 반환형을 가져야 하며, virtual 키워드는 선택사항이지만 명시하는 것이 좋다.
- 오버라이드하지 않으면, 기본 클래스의 함수가 그대로 상속된다.

아래 예제는 가상 함수의 사용 예시다:
```cpp
#include <iostream>

class Entity
{
protected:
    int x;
    int y;
public:
    Entity(int x, int y)
        : x{ x }, y{ y } {}
    virtual ~Entity() {
        std::cout << "Entity Destructor Called" << std::endl;
    }
    virtual void Move(int dx, int dy) {  // 가상 함수
        x += dx;
        y += dy;
    }
    void PrintPosition() {
        std::cout << x << "," << y << std::endl;
    }
};

class Player : public Entity
{
private:
    int hp;
    int xp;
public:
    Player(int x, int y, int hp, int xp)
        : Entity{ x, y }, hp{ hp }, xp{ xp } {}
    ~Player() {
        std::cout << "Player Destructor Called" << std::endl;
    }
    virtual void Move(int dx, int dy) {  // 오버라이드된 가상 함수
        x += dx * 2;
        y += dy * 2;
    }
};

int main()
{
    Player p{ 1, 1, 10, 10 };
    p.PrintPosition();
    p.Move(2, 1);
    p.PrintPosition();
}
```

#### 2.3 가상 소멸자
```cpp
int main()
{
    Entity* ptr = new Player{ 2, 3, 5, 5 };
    // 사용 후
    delete ptr; // ptr의 선언 타입이 Entity*이지만, virtual 소멸자 덕분에 Player의 소멸자도 호출됨.
}
```
- 다형성 객체를 소멸할 때, 기본 클래스 소멸자에 virtual이 없다면 기본 클래스의 소멸자만 호출되어 메모리 해제에 문제가 발생할 수 있다.
- 따라서 가상 소멸자를 함께 정의해야 한다.

#### 2.4 정리
- 기본 클래스의 함수를 virtual로 선언하면, 동적 바인딩이 가능해진다.
- 이 경우, 소멸자도 virtual로 선언하여 올바른 소멸 순서를 보장해야 한다.

## 3. 기본 클래스의 포인터/참조자

- C++에서 런타임 다형성을 구현하기 위해서는 아래 조건이 필요하다:
  - 상속
  - **기본 클래스 포인터 또는 참조자**
  - 가상 함수

#### 3.1 기본 클래스의 포인터 사용 예
```cpp
Entity *p1 = new Entity{0,0};
Entity *p2 = new Player{0,0,2,2};
Entity *p3 = new Enemy{0,0,2};   // Enemy 클래스 가정
Entity *p4 = new Boss{0,0,2};    // Boss 클래스 가정

// Entity::Move()가 virtual로 선언되었으므로:
p1->Move(1,1);  // Entity::Move()
p2->Move(1,1);  // Player::Move()
p3->Move(1,1);  // Enemy::Move()
p4->Move(1,1);  // Boss::Move()
```
이제 배열에 저장하여 사용할 수 있다:
```cpp
Entity *array[] = { p1, p2, p3, p4 };

for (int i = 0; i < 4; i++)
    array[i]->Move(1, 1);
```

#### 3.2 기본 클래스의 참조자 사용 예
```cpp
int main()
{
    Player p{ 3, 3, 20, 20 };
    Entity& ref = p;
    ref.Move(2, 2);       // Player::Move() 호출
    ref.PrintPosition();  
}
```
- 기본 클래스 참조자를 사용하면 동적 바인딩이 발생한다.

또한:
```cpp
void MoveX(Entity& e, int dx)
{
    e.Move(dx, 0);
}

Entity e{0,0};
MoveX(e, 1); // Entity::Move() 호출

Player p{0,0,2,2};
MoveX(p, 1); // Player::Move() 호출
```

#### 3.3 정리
- 동적 바인딩을 위해 기본 클래스 포인터 또는 참조자를 사용해 유도 클래스를 가리키고, 함수를 호출해야 한다.

## 4. override/final 지정자

#### 4.1 override 지정자
- 기본 클래스의 가상 함수를 오버라이드할 때, 함수의 원형과 반환형이 동일해야 한다.
  - 만약 다르다면, 오버라이드가 아닌 별개의 함수로 인식되어 정적 바인딩된다.
- C++11부터 override 지정자를 사용하면 오버라이드 여부를 컴파일러가 체크해 실수를 방지할 수 있다.
```cpp
#include <iostream>

class Entity
{
protected:
    int x;
    int y;
public:
    Entity(int x, int y) : x{ x }, y{ y } {}
    virtual ~Entity() {
        std::cout << "Entity Destructor Called" << std::endl;
    }
    virtual void Move(int dx, int dy)
    {
        x += dx;
        y += dy;
    }
    virtual void PrintPosition() const
    {
        std::cout << "Entity: " << x << "," << y << std::endl;
    }
};

class Player : public Entity
{
private:
    int hp;
    int xp;
public:
    Player(int x, int y, int hp, int xp)
        : Entity(x, y), hp{ hp }, xp{ xp } {}
    ~Player() {
        std::cout << "Player Destructor Called" << std::endl;
    }
    virtual void Move(int dx, int dy) // 오버라이드된 가상 함수
    {
        x += dx * 2;
        y += dy * 2;
    }
    virtual void PrintPosition() const override
    {
        std::cout << "Player: " << x << "," << y << std::endl;
    }
};

int main()
{
    Player p{ 1, 1, 10, 10 };
    const Entity& e = p;
    e.PrintPosition();
}
```
```cpp
// 아래와 같이 override 지정자를 사용하지 않으면,
// 오버라이드 여부를 파악하기 어려울 수 있다.
virtual void PrintPosition();
// 대신
virtual void PrintPosition() const override;
```

#### 4.2 final 지정자
- C++11부터 final 지정자를 통해 클래스나 멤버 함수가 더 이상 상속 또는 오버라이드되지 못하도록 할 수 있다.

##### 4.2.1 클래스의 final
```cpp
class Base final {
    // ...
};

class Derived : public Base {   // ERROR: Base는 final 클래스이므로 상속 불가
};
```

##### 4.2.2 함수의 final
```cpp
class A {
public:
    virtual void doSomething();
};

class B : public A {
public:
    virtual void doSomething() override final;
};

class C : public B {
public:
    virtual void doSomething(); // ERROR: B의 doSomething()은 final
};
```

#### 4.3 정리
- **override**: 가상 함수 오버라이드를 명시하여 실수를 방지한다.
- **final**: 클래스나 함수가 더 이상 상속/오버라이드되지 않도록 한다.

## 5. 순수 가상 함수와 추상 클래스

#### 5.1 추상 클래스 개념
- **추상 클래스 (Abstract Class)**: 객체를 생성할 수 없는 클래스이며, 상속 계층에서 기본 클래스로 사용된다.
  - 예: Entity 클래스
- **구상 클래스 (Concrete Class)**: 객체를 생성할 수 있으며, 모든 멤버 함수가 구현된 클래스이다.

#### 5.2 순수 가상 함수
- 멤버 함수 선언 뒤에 `= 0`을 붙이면 순수 가상 함수가 된다.
- 순수 가상 함수가 있는 클래스는 추상 클래스가 되며, 유도 클래스는 반드시 이를 오버라이드해야 한다.
```cpp
virtual void function() = 0;
```

#### 5.3 사용 목적
- 기본 클래스에서의 구현이 적절하지 않거나 어려운 경우, 유도 클래스에서 반드시 구현하도록 강제하기 위해 사용된다.

#### 5.4 사용 예시
```cpp
class Shape {   // 추상 클래스
public:
    virtual void draw() = 0;    // 순수 가상 함수
    virtual void rotate() = 0;
    virtual ~Shape();
};
```

```cpp
class Circle : public Shape {
public:
    virtual void draw() override {
        // 원을 그리는 구현
    }
    virtual void rotate() override {
        // 원을 회전시키는 구현
    }
    virtual ~Circle();
};
```
- 추상 클래스는 객체를 생성할 수 없지만, 포인터나 참조자를 통해 동적 바인딩이 가능하다.
```cpp
Shape *ptr = new Circle();
ptr->draw();
ptr->rotate();
```

##### 5.5 정리
- 순수 가상 함수가 있는 클래스는 추상 클래스이며, 이를 상속받은 구상 클래스는 순수 가상 함수를 오버라이드해야 객체 생성이 가능하다.

## 6. 인터페이스 클래스

#### 6.1 추상 클래스를 사용한 인터페이스 클래스
- **인터페이스 클래스**: 순수 가상 함수만을 가진 추상 클래스로, 클래스가 제공해야 할 기능(서비스)을 명세한다.
- 인터페이스 클래스를 상속받은 구상 클래스는 모든 순수 가상 함수를 구현해야 한다.
- 일반적으로 인터페이스 클래스의 이름은 대문자 I를 앞에 붙인다.

```cpp
class IShape {   // 인터페이스 클래스 (추상 클래스)
public:
    virtual void draw() = 0;
    virtual void rotate() = 0;
    virtual ~IShape();
};
```

```cpp
class Circle : public IShape {   // 구상 클래스: 인터페이스를 구현
public:
    virtual void draw() override {
        // 원을 그리는 구현
    }
    virtual void rotate() override {
        // 원을 회전시키는 구현
    }
    virtual ~Circle();
};
```

---

### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했음.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_

