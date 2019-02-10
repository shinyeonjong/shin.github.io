---
layout: post
title: "전략 패턴"
description: "strategy pattern"
date: 2019-02-09
tags: [strategy, pattern]
comments: true
share: true
---

## 1. 전략 패턴 [Strategy Pattern]
> 전략 패턴 이란 행동은 같으나 행위가 다를때 상속이 아닌 ```구성```을 활용하는 패턴이다.
    
**왜 상속이 아닌 구성을 활용할까?**      
상속을 활용하면 불필요한 상위 프로퍼티들을 가지고 있을 수 있고 기능이 추가, 수정 될 때마다 많은 부분을 수정해야 하기 때문이다.

---   
변하는 부분과 변하지 않는 부분을 분리하고, 바뀌는 부분은 따로 뽑아서 캡슐화시킨다. 캡슐화하여 교환해서 사용할 수 있도록 만든다.
![Large example image](/images/strategy_pattern.png "Large example image"){: .center-image}

##### 캐릭터 구조
```java
public abstract class Character {
    AttackBehavior attackBehavior;
    DefendBehavior defendBehavior;

    public Character() {
        this.introduce();
    }

    public void performedAttack() {
        this.attackBehavior.attack();
    }

    public void performedDefend() {
        this.defendBehavior.defend();
    }

    // 동적으로 행위를 변경하기 위해 각 전략의 setter 메소드를 작성한다.
    public void setAttackStrategy(AttackBehavior attackStrategy) {
        this.attackBehavior = attackStrategy;
    }

    public void setDefendBehavior(DefendBehavior defendBehavior) {
        this.defendBehavior = defendBehavior;
    }

    abstract void introduce();
}

```
##### 다양한 캐릭터들
```java
public class User extends Character {

    public User() {
        super.attackBehavior = new AttackWithFist();
        super.defendBehavior = new DefendWithArmor();
    }

    @Override
    void introduce() {
        System.out.println("[초보]");
    }
}

public class NPC extends Character {

    public NPC() {
        super.attackBehavior = new AttackWithFist();
        super.defendBehavior = new DefendWithArmor();
    }

    @Override
    void introduce() {
        System.out.println("[NPC]");
    }
}

public class Master extends Character {

    public Master() {
        super.attackBehavior = new AttackWithGun();
        super.defendBehavior = new DefendWithGhillieSuit();
    }

    @Override
    void introduce() {
        System.out.println("[만렙]");
    }
}
```
##### 공격 행동 정의
```java
public interface AttackBehavior {
    void attack();
}

public class AttackWithFist implements AttackBehavior {

    @Override
    public void attack() {
        System.out.println("불주먹~");
    }
}

public class AttackWithGun implements AttackBehavior {

    @Override
    public void attack() {
        System.out.println("탕탕~");
    }
}
```
##### 방어 행동 정의
```java
public interface DefendBehavior {
    void defend();
}

public class DefendWithGhillieSuit implements DefendBehavior {

    @Override
    public void defend() {
        System.out.println("길리슈트 착용");
    }
}

public class DefendWithArmor implements DefendBehavior {

    @Override
    public void defend() {
        System.out.println("방탄복 착용");
    }
}
```
##### 실행
```java
public class Execute {
    public static void main(String[] args) {
        Character npc = new NPC();
        Character master = new Master();
        Character user = new User();
        
        npc.performedAttack();
        npc.performedDefend();
        user.performedAttack();
        user.performedDefend();
        // 실행중 행위 변경이 가능함
        user.setAttackStrategy(new AttackWithGun());
        user.performedAttack();
        master.performedAttack();
        master.performedDefend();
    }
}
```