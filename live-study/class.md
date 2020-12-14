# 5주차: 클래스

## 학습할 것

- [클래스 정의하는 방법](#클래스-정의하는-방법)
- [객체 만드는 방법 (new 키워드 이해하기)](#객체-만드는-방법-new-키워드-이해하기)
- [메소드 정의하는 방법](#메소드-정의하는-방법)
- [생성자 정의하는 방법](#생성자-정의하는-방법)
- [this 키워드 이해하기](#this-키워드-이해하기)

## 과제(선택)

- [int 값을 가지고 있는 이진 트리를 나타내는 Node 라는 클래스를 정의하세요.](#int-값을-가지고-있는-이진-트리를-나타내는-node-라는-클래스를-정의하세요)
- [int value, Node left, right를 가지고 있어야 합니다.](#int-value-node-left-right를-가지고-있어야-합니다)
- [BinrayTree라는 클래스를 정의하고 주어진 노드를 기준으로 출력하는 bfs(Node node)와 dfs(Node node) 메소드를 구현하세요.](#binraytree라는-클래스를-정의하고-주어진-노드를-기준으로-출력하는-bfsnode-node와-dfsnode-node-메소드를-구현하세요)
- [DFS는 왼쪽, 루트, 오른쪽 순으로 순회하세요.](#dfs는-왼쪽-루트-오른쪽-순으로-순회하세요)

## 클래스 정의하는 방법

객체지향 프로그래밍(OOP)에서 가장 기본이 되는 Class는 다음과 같이 정의할 수 있습니다.

클래스는 필드를 선언할 수 있고, 생성자를 가져야 하며, 메서드를 선언할 수 있습니다.

```java
public class Bicycle {

  // Bicycle 클래스는 3개의 필드를 가지고 있습니다.
  // 클래스에 선언된 필드와 함수들을 통틀어 멤버라고 합니다.
  // 아래의 필드는 멤버 변수라고도 부르고, 속성(property)이라고도 합니다,
  // 일반적으로 객체지향 프로그래밍을 할 때는 private 접근제어자로 속성을 감춥니다.
  public int cadence;
  public int gear;
  public int speed;

  // 다음은 생성자라고 불리는 문법입니다.
  // 생성자는 특수한 형태로, 인스턴스를 초기화 할 때 사용하는 문법입니다.
  // 생성자가 만약 존재하지 않는다면 기본 생성자(매개변수를 받지 않는)가 생성됩니다.
  // 생성자는 클래스와 이름이 같아야 합니다.
  // this 키워드는 생성되는 인스턴스를 가리킵니다. (혼란을 방지하기 위해 사용합니다.)
  public Bicycle(int startCadence, int startSpeed, int startGear) {
    this.cadence = startCadence;
    this.speed = startSpeed;
    this.gear = startGear;
  }

  // Bicycle 클래스는 4개의 메서드를 가집니다.
  public void setCadence(int newValue) {
    this.cadence = newValue;
  }

  public void setGear(int newValue) {
    this.gear = newValue;
  }

  public void applyBrake(int decrement) {
    this.speed -= decrement;
  }

  public void speedUp(int increment) {
    this.speed += increment;
  }
}
```

이 코드의 마지막 메서드 두 개는 객체지향적이라고 볼 수 있습니다. 외부에서 setter로 속도를 조절하는 것이 아니라, 객체가 직접 자신의 속성을 의미있게 변경하도록 했기 때문입니다.

이런 식의 코드를 지향한다면, 보다 읽기좋고, 변화에 유연한 코드를 작성할 수 있습니다.

### 참고

- [오라클 자바 튜토리얼(클래스)](https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html)
- [생성자와 메서드의 차이](https://www.tutorialspoint.com/Difference-between-constructor-and-method-in-Java)

## 객체 만드는 방법 (new 키워드 이해하기)

## 메소드 정의하는 방법

## 생성자 정의하는 방법

## this 키워드 이해하기

## int 값을 가지고 있는 이진 트리를 나타내는 Node 라는 클래스를 정의하세요.

## int value, Node left, right를 가지고 있어야 합니다.

## BinrayTree라는 클래스를 정의하고 주어진 노드를 기준으로 출력하는 bfs(Node node)와 dfs(Node node) 메소드를 구현하세요.

## DFS는 왼쪽, 루트, 오른쪽 순으로 순회하세요.
