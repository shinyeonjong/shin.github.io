---
layout: post
title: "옵저버 패턴"
description: "observer pattern"
date: 2018-12-22
tags: [observer, pattern]
comments: false
share: false
---

## Observer Pattern [옵저버 패턴]
 >옵저버 패턴에서는 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들한테 연락이 가고 자동으로 내용이 갱신되는 방식으로 **일대다(one-to-many)** 의존성을 정의한다.  
 
 
 일대다 관계는 주제와 옵저버에 의해 정의된다. 옵저버는 주제에 의존한다. 주제의 상태가 바뀌변 옵저버한테 연락이 가고 연락 방법에 따라 옵저버에 있는 값이 새로운 값으로 갱신될 수도 있다.
 
#### 느슨한 결합(Loose coupling)의 위력
 >디자인원칙     
 서로 상호작용을 하는 객체 사이에는 가능하면 느슨하게 결합하는 디자인을 사용해야 한다.           
 
두 객체가 느슨하게 결합되어 있다는 것은, 그 둘이 상호작용을 하긴 하지만, 서로에 대해 서로 잘 모른다는 것을 의미한다.        
옵저버 패턴에서는 주제와 옵저버가 느슨하게 결합되어 있는 객체 디자인을 제공한다.**왜 그럴까??**     
- 주제가 옵저버에 대해서 아는 것은 옵저버가 특정 인터페이스(Observer 인터페이스)를 구현한다는 것 뿐이다.
- 옵저버는 언제든지 새로 추가할 수 있다.
- 새로운 형식의 옵저버를 추가하려고 할 때도 주제를 전혀 변경할 필요가 없다.
- 주제와 옵저버는 서로 독립적으로 재사용할 수 있다.
- 주제나 옵저버가 바뀌더라도 서로한테 영향을 미치지는 않는다.


#### 자바 내장 기능 옵저버
 **java.util.Observable의 단점**     
 Observable은 클래스다. 구현이 아닌 인터페이스에 맞춰서 프로그래밍한다는 객체지향 디자인 원칙에 위배되는것이 아닌가???
  
  1. Observable이 클래스기 떄문에 서브클래스를 만들어야 한다는 점. 이미 다른 수퍼클래스를 확장하고 있는 클래스에 Observable의 기능을 추가할 수 없다. 그래서 재사용성에 제약이 생긴다.
  2. ```setChanged()```메소드가 ```protected```로 선언되어 있기 때문에 서브클래스에서만 호출 가능하다. 
 
 - JavaBeans와 Swing에서도 패턴을 구현한 것을 제공한다.

##### Swing으로 보는 간단한 예제
```java
public class SwingObserverExample {
    private JFrame frame;

    public static void main(String[] args) {
        SwingObserverExample example = new SwingObserverExample();
        example.go();
    }

    public void go() {
        this.frame = new JFrame();
        JButton button = new JButton("정말 해도 될까?");
        // AngeListener와 DevilListener를 버튼의 리스너(옵저버)로 만든다.
        button.addActionListener(new AngeListener());
        button.addActionListener(new DevilListener());
        this.frame.getContentPane().add(BorderLayout.CENTER, button);
        this.frame.setSize(200, 200);
        this.frame.setVisible(true);
    }
}
```
```java
public class AngeListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("안돼, 분명 후화할까여");
    }
}
```
```java
public class DevilListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("그래그래!!!!!!!!!!!!!!!!!!!!!!!");
    }
}
```