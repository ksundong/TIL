# 9주차 과제: 예외 처리

## 학습할 것

- [예외란](#예외란)
- [자바에서 예외 처리 방법 (try, catch, throw, throws, finally)](#자바에서-예외-처리-방법-try-catch-throw-throws-finally)
- [try-with-resources](#try-with-resources)
- [자바에서 예외를 발생시키는 방법](#자바에서-예외를-발생시키는-방법)
- [자바가 제공하는 예외 계층 구조](#자바가-제공하는-예외-계층-구조)
- [Exception과 Error의 차이는?](#exception과-error의-차이는)
- [RuntimeException과 RE가 아닌 것의 차이는?](#runtimeexception과-re가-아닌-것의-차이는)
- [커스텀한 예외 만드는 방법](#커스텀한-예외-만드는-방법)
- [예외 포장](#예외-포장)

## 예외란

예외는 "exceptional event"의 약어입니다.

정의: 예외는 프로그램 실행중에 발생하는 프로그램의 실행의 일반적인 흐름을 방해하는 이벤트입니다.

메서드내에서 에러가 발생하면, 메서드는 객체를 만들고 런타임 시스템에 전달합니다. 이 객체는 예외 객체라고 불리며 에러에 대한 정보와 이에 대한 타입, 에러가 발생한 시점의 프로그램의 상태에 대한 정보를 담고있습니다. 예외 객체를 생성하여 런타임 시스템에 전달하는 것을 예외를 던진다고 표현합니다.

메서드가 예외를 던지면, 런타임 시스템은 해결할 수 있는 '무엇'을 찾기위해 시도합니다. 예외를 처리할 수 있는 '무엇'의 집합은 오류가 발생한 메서드를 사용하기 위해 불려진 메서드의 순서가 있는 리스트입니다. 메서드의 리스트는 `콜 스택`이라고 알려져 있습니다.

![콜 스택](https://docs.oracle.com/javase/tutorial/figures/essential/exceptions-callstack.gif)

**콜 스택**

이미지 출처: <https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html>

런타임 시스템은 예외를 제어할 수 있는 코드 블럭을 가지고 있는 메서드를 찾기위해 콜스택에서 검색합니다. 이 코드 블럭을 `exception handler`라고 부릅니다. 검색은 오류가 발생한 메서드로부터 시작하여 메서드가 호출된 역순으로 콜 스택을 통해서 진행됩니다. 적절한 핸들러가 발견되면, 런타임 시스템은 예외를 핸들러로 전달합니다. `exception handler`는 던져진 예외 오브젝트의 타입이 핸들러가 제어할 수 있는 타입과 일치하는 경우 적절하다고 간주됩니다.

선택된 `exception handler`는 예외를 잡았다고 말합니다. 만약에 런타임 시스템이 적절한 `exception handler`를 찾지 못하고 콜 스택의 모든 메서드를 철저하게 검색하면, 런타임 시스템(결과적으로 프로그램)은 종료됩니다. (이는 비정상 종료로 볼 수 있습니다.)

![예외 처리기를 찾기 위해 콜 스택을 찾는 과정](https://docs.oracle.com/javase/tutorial/figures/essential/exceptions-errorOccurs.gif)

**`exception handler`를 찾기 위해 콜 스택을 찾는 과정**

이미지 출처: <https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html>

예외를 사용하여 에러를 관리하면 기존 에러관리 기술에 비해 몇 가지 장점이 있습니다.

## 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)

## try-with-resources

## 자바에서 예외를 발생시키는 방법

## 자바가 제공하는 예외 계층 구조

## Exception과 Error의 차이는?

## RuntimeException과 RE가 아닌 것의 차이는?

### Checked Exception, Unchecked Exception

## 커스텀한 예외 만드는 방법

## 예외 포장
