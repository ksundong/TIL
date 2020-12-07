# 4주차: 제어문

## 학습할 것

- [선택문](#선택문)
- [반복문](#반복문)

## 과제(선택)

- [JUnit5 학습하기](#junit5-학습하기)
- [live-study 대시보드 만들기](#live-study-대시보드-만들기)
- [LinkedList 구현하기](#linkedlist-구현하기)
- [Stack 구현하기](#stack-구현하기)
- [ListNode로 Stack 구현하기](#listnode로-stack-구현하기)
- [Queue 구현하기](#queue-구현하기)

---

일반적으로 우리가 작성한 소스코드는 위에서 아래로 실행됩니다. 그러나, 우리가 앞으로 학습할 제어문은 실행 흐름을 끊고, 조건에 따라 실행되거나, 반복해서 실행되거나, 분기처리를 통해 우리의 프로그램이 조건에 따라 실행될 수 있도록 해줍니다.

자바에는 조건에 따라 실행되도록 하는 문법인 선택문(`if-then`, `if-then-else`, `switch`), 반복해서 실행되도록 하는 문법인 반복문(`for`, `while`, `do-while`)이 있으며, 분기를 수행하는 문법(`break`, `continue`, `return`) 또한 있습니다.

[자바 튜토리얼](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/flow.html)

## 선택문

### `if-then`, `if-then-else` 문

#### `if-then` 문

`if-then`문은 가장 기본적인 제어문입니다. 이 코드는 블록 안에 있는 코드를 오직 조건이 충족될 때만 실행됨을 알려줍니다.  
예를 들어, `Bicycle` 클래스는 자전거가 움직일 때만 속도를 줄일 수 있도록 합니다.

```java
void applyBrakes() {
  // 자전거는 움직이고 있어야만 합니다.
  if (this.isMoving) {
    // then 절입니다. 현재 속도를 줄입니다.
    this.currentSpeed--;
  }
}
```

이 평가가 `false` 값이라면 자전거가 움직이지 않고 있다는 것을 의미하고, 제어는 `if-then` 문 끝으로 이동하게 됩니다.

추가로, `then` 절이 하나의 문만 가질 경우 시작과 끝의 중괄호는 선택적입니다.

하지만 저는 개인적으로 모든 if문에 괄호를 사용하는 것을 선호합니다. 일반적으로 코드의 취약성이 높아지는 smell이라고 생각하며, 문장이 두 줄이 될 경우 중괄호를 추가해주어야 할 뿐더러 잊기도 쉽습니다.

```java
if (this.isMoving)
    this.currentSpeed--;
// 만약 한 문장이 더 추가되어야 한다면?
// 중괄호도 추가해주어야 함으로 유지보수성이 떨어지게 됩니다.
```

#### `if-then-else` 문

`if-then-else` 문은 `if` 절의 평가가 `false`인 경우에 실행될 두번째 경로를 제공해줍니다. 아래의 예제는 이미 자전거가 멈춰있는 경우에 이미 멈춰있다는 에러 메시지를 출력합니다.

```java
void applyBrakes() {
  if (this.isMoving) {
    this.currentSpeed--;
  } else {
    System.err.println("자전거가 이미 멈춰있습니다!");
  }
}
```

### `switch` 문

## 반복문

## JUnit5 학습하기

## live-study 대시보드 만들기

## LinkedList 구현하기

## Stack 구현하기

## ListNode로 Stack 구현하기

## Queue 구현하기
