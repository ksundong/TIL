# 15주차: 람다식

## 학습할 것

- [람다식 사용법](#람다식-사용법)
- [Functional Interface](#functional-interface)
- [Variable Capture](#variable-capture)
- [메소드, 생성자 레퍼런스](#메소드-생성자-레퍼런스)

## 람다식이란?

람다식은 단일 메서드를 가진 클래스의 인스턴스를 익명클래스를 사용하는 것 보다 간결하게 표현할 수 있도록 해줍니다.

보다 일반적인 람다식의 의미는 메서드를 하나의 식으로 표현한 것으로, 함수를 간략하고도 명확한 식으로 표현할 수 있도록 합니다. 메서드를 람다식으로 표현하면, 메서드의 이름과 반환값이 없어지므로 람다식을 익명 함수(anonymous function)이라고도 부릅니다.

람다식이 가져다 주는 장점은 또 있는데, 메서드를 선언하려면 자바에서는 클래스 혹은 인터페이스를 선언해야 하며, 인스턴스를 생성해야 메서드를 호출할 수 있습니다. 이를 간단히 한 것이 바로 람다식입니다.

람다식은 람다식 자체로 메서드의 매개변수가 될 수 있고, 메서드의 결과로 반환될 수 있다는 것이 핵심이며, 이를 first-class function이라고 부릅니다. 이는 함수형 프로그래밍에 필수적입니다.

물론 자바는 함수형 패러다임을 설계단계에서부터 고려한 다른 언어들과는 다르게 순수한 객체지향 언어로 설계되었으며, 함수형 패러다임의 이점을 잘 가져오기 위해 많은 노력이 들어간 결과물이 바로 자바의 람다식입니다.

### 참고

- [오라클 자바 튜토리얼(람다식)](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)
- 자바의 정석(람다식)
- [위키피디아(first-class function)](https://en.wikipedia.org/wiki/First-class_function)

## 람다식 사용법

## Functional Interface

## Variable Capture

## 메소드, 생성자 레퍼런스
