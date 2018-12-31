---
layout: post
title: "데코레이터 패턴"
description: "decorator pattern"
date: 2018-12-22
tags: [decorator, pattern]
comments: false
share: false
---
## Decorator Pattern [데코레이터 패턴]
>객체에 추가 요소를 동적으로 더할 수 있다. 데코레이터를 사용하면 서브클래스를 만드는 경우에 비해 훨씬 유연하게 기능을 확장할 수 있다.   

음료를 나타내는 추상 클래스
```java
public abstract class Beverage {
    private String description;

    public abstract int cost();

    public String getDescription() {
        return this.description;
    }
}
```
다양한 커피 메뉴 출시
```java
public class DarkMocha extends Beverage {

    @Override
    public int cost() {
        return 5500;
    }
}

public class DarkRoastWithSteamedMilkAndCaramel extends Beverage {

    @Override
    public int cost() {
        return 6700;
    }
}
```
문제1. 커피 종류가 많아질수록, 음료의 재료 가격이 달라질수록 클래스를 관리하는 게 만만치 않을 것이다.

구성과 위임을 통해서 실행중에 행동을 ```상속```시키는 방법이 있다.
서브클래스를 만드는 방식으로 행동을 상속 받으면 그 행동은 컴파일시에 완전히 결정된다. 게다가 모든 서브클래스에서 똑같은 행동을 상속 받아야 한다. 하지만 구성을 통해서 객체의 행동을 확장하면 실행중에 동적으로 행동을 설정할 수 있다.


#### OCP(Open-Closed Principle)         
>디자인 원칙     
클래스는 확장에 대해서는 열려 있어야 하지만 코드는 변경에 대해서는 닫혀 있어야 한다.        

#### 데코레이터 패턴의 정의

* 데코레이터의 수퍼클래스는 자신이 장식하고 있는 객체의 수퍼클래스와 같다.
* 한 객체를 여러 개이 데코레이터로 감쌀 수 있다.
* 데코레이터는 자신이 감싸고 있는 객체와 같은 수퍼클래스를 가지고있기 때문에 원래 객체가 들어갈 자리에 데코레이터 객체를 집어넣어도 상관 없다.        
* 데코레이터는 자신이 장식하고 있는 객체에게 어떤 행동을 위임하는 것 외에 원하는 추가적인 작업을 수행할 수 있다.
* 객체는 언제든지 감쌀 수 있기 때문에 실행중에 필요한 데코레이터를 마음대로 적용할 수 있다.


>Beverage 라는 추상클래스의 서브클래스를 만드는 게 행동을 상속 받음이 아니라 올바른 형식을 맞추기 위해서다.
객체 구성(인스턴스 변수로 다른 객체를 저장하는 방식)을 이용하고 있기 때문에 음료하고 첨가물을 다향하게 섞어도 유연성을 잃지 않을 수 있다.

```java
public abstract class Beverage {
    private String description;

    public String getDescription() {
        return this.description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public abstract int cost();

}
```
```java
/*
데코레이터 패턴에서는 상속을 이용해 형식을 맞추는것, 행동을 물려받는것은 아님
 */
public abstract class CondimentDecorator extends Beverage {
    public abstract String getDescription();
}
```
음료 구현
```java
public class Espresso extends Beverage {
    public Espresso() {
        super.setDescription("에스프레소");
    }

    @Override
    public int cost() {
        return 4000;
    }
}
```
첨가물 구현
```java
public class Mocha extends CondimentDecorator {
    //객체 구성(인스턴스 변수로 다른 객체를 저장하는 방식)
    private Beverage beverage;

    public Mocha(Beverage beverage) {
        this.beverage = beverage;
    }

    @Override
    public String getDescription() {
        return this.beverage.getDescription() + " 모카 추가";
    }

    @Override
    public int cost() {
        return this.beverage.cost() + 500;
    }
}

public class Milk extends CondimentDecorator {
    //객체 구성(인스턴스 변수로 다른 객체를 저장하는 방식)
    private Beverage beverage;

    public Milk(Beverage beverage) {
        this.beverage = beverage;
    }

    @Override
    public String getDescription() {
        return this.beverage.getDescription() + " 우유 추가";
    }

    @Override
    public int cost() {
        return this.beverage.cost() + 1000;
    }
}
```
실행
```java
public class Cafe {
    public static void main(String[] args) {
        Beverage houseBlend = new Milk(new Mocha(new HouseBlend()));
        System.out.println(houseBlend.getDescription());
        System.out.println(houseBlend.cost() + "원");
    }
}
/*
결과
하우스블렌디드 모카 추가 우유 추가
6500원
 */
```