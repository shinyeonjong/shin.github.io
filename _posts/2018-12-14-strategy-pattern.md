---
layout: post
title: "strategy pattern"
description: "strategy pattern"
date: 2018-12-14
tags: [strategy, pattern]
comments: false
share: false
---

## 1. Strategy Pattern [전략 패턴]

- ```객체지향 기법```을 사용하여 Duck 이라는 수퍼클래스를 만든다음, 그 클래스를 확장하여 다른 모든 종류의 오리를 만든다. (상속)          
수정사항 1. 오리가 날아야해요.      
```java
public abstract class Duck {
    public void quack() {
        System.out.println("꽦!!");
    }

    public void swim() {
        System.out.println("헤엄헤엄~");
    }

    public abstract void display();
    
    // 오리들이 날아야하는 문제가 생겨 추가
    // *템플릿 메소드 패턴(Template Method Pattern) : 공통인 부분을 추출하여 뺌
    public void fly() {
        System.out.println("날아라~");
    }
}
```
수정사항2. 유인용 새는 울지도 날아도 안 돼요.     
```java
public class DecoyDuck extends Duck {

    @Override
    public void display() {
        System.out.println("유인용 새입니다.");
    }

    // 유인용 새는 울 수 없어요.
    @Override
    public void quack() {
        System.out.println("");
    }
    
    // 유인용 새는 날 수 없어요.
    @Override
    public void fly() {
        System.out.println("");
    }
}
```
수정사항3. 고무오리는 날면 안돼요.        
```java
public class RubberDuck extends Duck {

    @Override
    public void display() {
        System.out.println("고무오리야");
    }

    // 고무오리는 삒삒 울어요.
    @Override
    public void quack() {
        System.out.println("삒삒");
    }

    // 고무오리는 날 수 없어요.
    @Override
    public void fly() {
        System.out.println("");
    }
}
```
```상속```을 사용한다면 서브클래스마다 오리의 행동이 바뀔 수 있는데도 모든 서브클래스에서 한 행동을 사용하도록 하는 것은 올바르지 않다.
이렇게 오리가 하나 추가될 때마다, 기능이 추가될 때마다 많은 부분을 수정해야 한다.
#### 인터페이스는 어떨까요?       
```java
public interface Quackable {
    void quack();
}

public interface Flyable {
    void fly();
}
```

```java
public class MallardDuck extends Duck implements Flyable, Quackable {

    @Override
    public void display() {
        System.out.println("나는 청둥오리야");
    }

    // 중복
    @Override
    public void fly() {
        System.out.println("오리는 날아요.");
    }

    @Override
    public void quack() {
        System.out.println("천둥오리 : 꽥~");
    }
}
```
```java
public class RedHeadDuck extends Duck implements Flyable, Quackable {

    @Override
    public void display() {
        System.out.println("나는 아메리카흰죽지..?야");
    }

    // 중복
    @Override
    public void fly() {
        System.out.println("오리는 날아요.");
    }

    @Override
    public void quack() {
        System.out.println("아메리카흰죽지 : 꽉꽉");
    }
}
```
인터페이스에는 구현된 코드가 없기때문에 ```코드 재사용```을 할수 없다는 문제점이 있다.         
수정사항4. 중복을 제거하세요.
>디자인원칙  
애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리한다.   
'바뀌는 부분은 따로 뽑아서 캡슐화시킨다. 그렇게 하면 나중에 바뀌지 않는 부분에는 영향을 미치지 않은 채로 그 부분만 고치거나 확장할 수 있다.'

```java
public class FlyNoWay implements FlyBehavior {
    @Override
    public void fly() {
        // 날지 않아요.
    }
}

public class FlyWithWings implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("날개를 이용해 푸드덕");
    }
}
```
```java
public class Quack implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("꽥");
    }
}

public class Squeak implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("삑삑");
    }
}

public class MuteQuack implements QuackBehavior {
    @Override
    public void quack() {
        //울지 않아요.
    }
}
```

#### Duck의 행동 구현
1. Duck 클래스에 ```flyBehavior```, ```quackBehavior``` 두 개의 인터페이스 형식의 인터페이스 변수를 추가하고
2. ```fly()```와 ```quack()``` 대신 ```performQuack()``` 와 ```performFly()``` 메소드를 추가한다.
```java
public abstract class Duck {
    // 1.
    QuackBehavior quackBehavior;
    FlyBehavior flyBehavior;

    // 2.
    public void performQuack() {
        // 꽥꽥거리는 행동을 직접 처리하는 대신, quackBehavior로 참조되는 객체에 행동을 위임한다.
        this.quackBehavior.quack();
    }

    // 2.
    public void performFly() {
        this.flyBehavior.fly();
    }

    public void swim() {
        System.out.println("헤엄헤엄~");
    }

    public abstract void display();
}
```

3. 각 오리 객체에서는 실행시에 ```flyBehavior```, ```quackBehavior``` 두 변수에 특정 행동 형식에 대한 레퍼런스를 다형적으로 설정한다. 
```java
public class MallardDuck extends Duck {
    // 1) MallardDuck의 인스턴스가 만들어질 때
    public MallardDuck() {
        // 2) 생성자에서는 Duck으로부터 상속받은 quackBehavior 인스턴스 변수에
        // Quack(QuackBehavior를 구현한 구상 클래스)형식의 새로운 인스턴스를 대입합니다.
        super.quackBehavior = new Quack();
        // 3) 나는 행동 또한 마찬가지 상속받은 flyBehavior 인스턴스 변수에
        // FlyWithWings(FlyBehavior를 구현한 구상 클래스)형식의 새로운 인스턴스를 대입합니다.
        super.flyBehavior = new FlyWithWings();
    }

    @Override
    public void display() {
        System.out.println("나는 청둥오리야");
    }
}
```
#### 동적으로 행동을 지정하는 방법
1. Duck클래스에 ```setter```메소드를 만든다.
```java
public void setFlyBehavior(FlyBehavior flyBehavior) {
    this.flyBehavior = flyBehavior;
}

public void setQuackBehavior(QuackBehavior quackBehavior) {
    this.quackBehavior = quackBehavior;
}
```
2. ```ModelDuck``` 예제. 
```java
public class DuckSimulator {
    public static void main(String[] args) {
        Duck mallardDuck = new MallardDuck();
        mallardDuck.performFly();
        mallardDuck.performQuack();

        Duck model = new ModelDuck();
        model.performFly();
        model.setFlyBehavior(new FlyRocketPowered());
        model.performFly();
    }
}
```
```java
//결과
날개를 이용해 푸드덕
꽥
날지 않아요.
로켓 추진으로 날아갑니다.
```

#### "A는 B이다." 보다 "A에는 B가 있다."가 나을 수 있습니다.
>디자인원칙  
상속보다는 구성을 활용한다.

Duck 에는 ```QuackBehavior```와```FlyBehavior```가 있다.
이런 식으로 하쳐지는 것을 ```구성(composition)```을 이용하는 것이라고 부른다.
구성을 이용하면 유연성을 크게 향상시킬 수 있다.
```java
public abstract class Duck {
    QuackBehavior quackBehavior;
    FlyBehavior flyBehavior;
    ...
```
#### 마지막 정리              

>Strategy Pattern   
스트래티지패턴에서는 알고리즘군을 정의하고 각각을 캡슐화하여 교환해서 사용할 수 있도록 만든다. 스트래티지 패턴을 활용하면 알고리즘을 사용하는 클라이언트와는 독립적으로 알고리즘을 변경할 수 있다.
