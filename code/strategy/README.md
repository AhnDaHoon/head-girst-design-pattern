# 전략 패턴

전략 패턴이란, 비슷한 동작을 하지만 다르게 구현되어 있는 행위(전략)들을 공통의 인터페이스를 구현하는 각각의 클래스로 구현하고, 동적으로 바꿀 수 있도록 하는 패턴이다. 전략 패턴으로 구현된 코드는 직접 행위에 대한 코드를 수정할 필요 없이 전략만 변경하여 유연하게 확장할 수 있게 된다.

## 전략 패턴 예제

오리가 나오는 게임에서 여러 오리를 구현 하려면 오리라는 추상 클래스가 있고 이 클래스로 여러 오리를 구현할 것이다. 구현 클래스로는 청둥오리, 장남감, 유인 오리 클래스가 있다.
<br/>

![https://user-images.githubusercontent.com/82018440/232795846-24b297ec-9697-48c6-8db2-3439504784ba.png](https://user-images.githubusercontent.com/82018440/232795846-24b297ec-9697-48c6-8db2-3439504784ba.png)

<br/>
그러나 이런식으로 상속을 하면 fly()와 quack() 메소드를 일일이 살펴보고 상황에 따라 오버라이드 해야한다. 만약 이 메서드들의 로직을 변경해야 한다면 상속 받은 모든 클래스들을 하나씩 변경해줘야 하는 일이 생긴다. 또 모든 서브 클래스에서 날거나 꽥꽥거리는 기능이 있어야 하는 것은 아니므로 상속이 올바른 방법이 아니다.
<br/><br/>
위에 문제를 해결 하려면 어떻게 해야 할까?
<strong>애플리케이션에서 바뀌는 부분을 따로 뽑아서 캡슐화 한다. 그러면 나중에 바뀌지 않는 부분에는 영향을 미치지 않고 그 부분만 고치거나 확장할 수 있다.</strong>

<br/>

![image](https://user-images.githubusercontent.com/82018440/233387186-f7f70d2b-956a-45ae-b0af-0c5764eff06e.png)

<br/>

이런 식으로 디자인하면 다른 형식의 객체에서도 나는 행동과 꽥꽥거리는 행동을 재사용할 수 있다. 그리고 기존의 행동 클래스를 수정하거나 날아다니는 행동을 사용하는 Duck 클래스를 전혀 건드리지 않고 새로운 행동을 추가할 수 있다.

<br/>

```java
public abstract class Duck {
	// 인터페이스를 캡슐화하여 상황에 따라 행동을 변경한다.
	FlyBehavior flyBehavior; 
	QuackBehavior quackBehavior;

	public Duck() {
	}

	public void setFlyBehavior(FlyBehavior fb) {
		flyBehavior = fb;
	}

	public void setQuackBehavior(QuackBehavior qb) {
		quackBehavior = qb;
	}

	abstract void display();

	public void performFly() {
		flyBehavior.fly();
	}

	public void performQuack() {
		quackBehavior.quack();
	}

	public void swim() {
		System.out.println("All ducks float, even decoys!");
	}
}
```
<br/>

```java
public class MallardDuck extends Duck {

	public MallardDuck() {
		// 클래스에 맞게 행동 인터페이스를 구현한 구현체 넣어준다.
		quackBehavior = new Quack();
		flyBehavior = new FlyWithWings();

	}

	public void display() {
		System.out.println("I'm a real Mallard duck");
	}
}
```
