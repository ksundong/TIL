# 호이스팅

자바스크립트에서는 변수 정의를 맨 위로 올려주는 호이스팅이라는 기능이 기본적으로 제공됩니다.

변수 정의를 하기 전에 변수 사용을 할 수 있다는 의미입니다. 다른 말로는 변수가 사용된 시점 이후에 변수 정의를 해줄 수 있습니다.

```jsx
x = 5;

elem = document.getElementById("demo");
elem.innerHTML = x;

var x;
```

이렇게 해도 코드는 동작합니다.

이를 아는 것이 바로 호이스팅을 아는 것 입니다.

호이스팅은 모든 선언을 현재 스코프(현재 스크립트 또는 현재 함수의)의 맨 위로 올리는 자바스크립트의 기본 동작입니다.

하지만 이는 `var` 일 때에만 이렇게 동작합니다.

`let` , `const` 에서는 맨 위에 선언은 할 수 있지만, 초기화 되지 않기 때문에 사용할 수 없습니다.

만약 `let` 변수를 사용하면 `ReferenceError` 가 발생하게 됩니다.

### 또 다른 case

```jsx
var x = 5;

elem = document.getElementById("demo");
elem.innerHTML = x + " " + y;

var y = 7;
```

이 코드에서는 `var y` 의 선언만 위로 올려지고 초기화 되지 않기 때문에 y의 값은 정의되지 않습니다.

즉, 이 코드는 다음과 같습니다.

```jsx
var x = 5;
var y;

elem = document.getElementById("demo");
elem.innerHTML = x + " " + y;

y = 7;
```

### 이런 호이스팅을 어떻게 대해야 할까요?

되도록이면 변수 선언을 위에 선언해두고 사용하는 것을 추천합니다.

만약, 호이스팅을 이해하지 못하는 경우나 간과하는 경우 프로그램에 버그가 포함될 수 있습니다.

### 참고

[JavaScript Hoisting](https://www.w3schools.com/js/js_hoisting.asp)

# 클로저

클로저는 함수와 함수가 선언된 어휘적 환경의 조합입니다. 이를 이해하기 위해서는 먼저 자바스크립트가 어떻게 변수의 유효범위를 지정하는지(Lexical scoping)을 먼저 이해해야 합니다.

## 어휘적 범위 지정(Lexical Scoping)

```jsx
function init() {
  var name = "Mozilla"; // name은 init에 의해 생성된 지역변수입니다.
  function displayName() { // displayName()은 내부 함수이며, 클로저입니다.
    alert(name);  // 부모 함수에서 선언된 변수를 사용할 수 있습니다.
  }
 displayName();
}
init();
```

`init()` 은 지역 변수 `name` 과 함수 `displayName()` 을 생성합니다. `displayName()` 은 내부함수이며 `init()` 의 body에서만 사용할 수 있습니다. 그리고 `displayName()` 내부에는 지역 변수가 없다는 점에 주의해야합니다. 하지만, 함수 내부에서 외부 함수의 변수에 접근 할 수 있기 때문에 `displayName()` 은 `name`에 접근할 수 있습니다. 만약, `displayName()` 에도 `name` 이라는 변수가 있었다면, `name` 은 내부에 선언된 변수를 사용했을 것입니다.

이 예시를 통해서 함수가 중첩된 상황에서 파서가 어떻게 변수를 처리하는지 알 수 있습니다. 이는 어휘적 범위 지정의 한 예입니다. 어휘적이라는 의미는 변수가 어디에서 사용 가능한지 알기 위해 그 변수가 소스코드 내 어디에서 선언되었는지 고려한다는 것을 의미합니다.

**중첩된 함수는 외부 스코프에서 선언한 변수에도 접근할 수 있습니다.**

Lexical Scope를 좀 더 잘 이해하려면 함수는 호출할 때가 아니라 어디에 선언했는지에 따라 스코프가 결정된다고 이해하면 좋습니다.

## 클로저란

자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, 외부 함수 밖에서 내부 함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데, 이런 함수를 클로저(Closure)라고 부릅니다.

## Closure

```jsx
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  return displayName();
}

var myFunc = makeFunc();
// myFunc는 displayName() 인스턴스가 됩니다.
// 어휘적 환경을 유지합니다.
myFunc(); // 리턴된 displayName() 함수를 실행합니다. (name 변수에 접근합니다.)
```

이 코드는 앞의 예제와 완전히 동일하게 동작합니다. 흥미로운 차이점은 `displayName()` 함수가 실행되기 전에 외부 함수인 `makeFunc()` 로 부터 리턴되어 `myFunc` 라는 변수에 저장된다는 것입니다.

이는 직관적이지 않게 보일 수 있습니다. 다른 언어에서는 지역 변수가 함수가 처리되는 동안에만 존재하기 때문입니다. 따라서 `makeFunc()` 가 종료되고 나서 더 이상 `name` 변수에 접근할 수 없을 것으로 예상하는 것이 일반적입니다.

자바스크립트는 함수르 리턴하고, 리턴하는 함수가 **클로저**를 형성합니다. 클로저는 함수와 함수가 선언된 어휘적 환경의 조합입니다. 이 환경은 클로저가 생성된 시점의 유효 범위 내에 있는 모든 지역 변수로 구성됩니다.

`myFunc` 는 `makeFunc` 가 실행될 때 생성된 `displayName` 함수의 인스턴스에 대한 참조입니다. `displayName` 의 인스턴스는 변수 `name` 이 있는 어휘적 환경에 대한 참조를 유지합니다. 따라서 "Mozilla"가 `alert` 에 전달됩니다.

좀 더 흥미로운 예제는 다음과 같습니다.

```jsx
function makeAdder(x) {
  var y = 1;
  return function(z) {
    y = 100;
    return x + y + z;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);
//클로저에 x와 y의 환경이 저장됨

console.log(add5(2));  // 107 (x:5 + y:100 + z:2)
console.log(add10(2)); // 112 (x:10 + y:100 + z:2)
//함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산
```

이 예제는 `makeAdder` 는 함수를 만들어내는 공장이라는 점을 잘 생각해보면 됩니다. 이는 `makeAdder` 가 특정한 값을 가질 수 있는 함수들을 리턴한다는 것을 의미합니다.

즉 `add5` 는 본질적으로

```jsx
function(z) {
  y = 100;
  return 5 + y + z;
}
```

와 같습니다.

`add5` 와 `add10` 은 둘 다 클로저라고 부릅니다. 이들은 같은 함수 본문의 정의는 공유하지만 서로 다른 맥락(어휘)적 환경을 저장합니다. `add5` 의 맥락적 환경에서 클로저 내부의 `x` 는 5이지만, `add10` 의 맥락적 환경에서는 `x` 가 10이 됩니다. 또한 내부에서 `y` 라는 외부에서 1로 초기화된 변수에 접근해서 100이라는 값을 할당해주었고, 이는 클로저가 리턴된 후에도 외부함수의 변수에 접근이 가능하다는 점을 알려줍니다. 즉, 단순히 값 형태로 변수가 전달되지 않는 것을 의미합니다.

## 클로저 유용하게 사용하기

- 이벤트 기반 프로그래밍(FE 에서 대화식 프로그램을 작성할 때도 사용합니다.)
- private 메서드 흉내내기(자바스크립트에는 태생적으로 `private` 이 없습니다.
  - 모듈 패턴
  - 정보 은닉, 캡슐화 등의 이점을 얻을 수 있습니다.

## 클로저 스코프 체인

모든 클로저에는 세가지 스코프가 있습니다.

- 지역 범위(Local Scope)
- 외부 함수 범위(Outer Functions Scope)
- 전역 범위(Global Scope)

우리는 세가지 범위 모두 접근할 수 있지만, 중첩된 내부 함수가 있는 경우 종종 실수를 저지를 수 있습니다. 이 부분은 잘 이해가 가지 않았습니다.

## 클로저 사용시 유의할 점

루프에서 클로저를 생성하면 조심해야할 점이 바로 스코프입니다.

```html
<p id="help">Helpful notes will appear here</p>
<p>E-mail: <input type="text" id="email" name="email"></p>
<p>Name: <input type="text" id="name" name="name"></p>
<p>Age: <input type="text" id="age" name="age"></p>
```

```jsx
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp();
```

이 코드를 보면, 잘 동작할 것 같지만 잘 동작하지 않습니다.

바로 클로저에서 사용하는 값이 전역변수이기 때문입니다. `let` 키워드를 사용하면 이 문제는 말끔히 해결됩니다.

### 참고

[클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)

[Closure | PoiemaWeb](https://poiemaweb.com/js-closure)

# this

자바스크립트의 `this` 키워드는 다른 언어와 조금 다르게 동작합니다. 또한 엄격 모드와 비엄격 모드에서도 차이가 있습니다.

대부분의 경우 `this` 의 값은 함수를 호출한 방법에 의해 결정됩니다. ES5에서는 함수를 어떻게 호출했는지 상관하지 않고 `this` 값을 설정할 수 있는 `bind` 메서드가 도입되었고, ES2015에서는 스스로의 `this` 바인딩을 제공하지 않는 화살표 함수를 추가했습니다.

웹 브라우저에서는 전역 문맥에서 `this` 가 곧 `window` 객체입니다. 이는 어떤 컨텍스트나 상관없이 `globalThis` 프로퍼티를 사용하여 전역객체를 얻을 수 있습니다.

## 함수 내에서

함수 내부에서 `this` 는 함수를 호출한 방법에 의해 좌우됩니다.

단순 호출의 경우 비엄격 모드에서는 함수내에서 `this` 를 사용하면 기본값으로 전역 객체를 참조합니다.

반면 엄격 모드에서는 문맥에 진입하며 설정되는 값을 유지하므로 `undefined` 를 참조합니다.

`this` 의 값을 한 문맥에서 다른 문맥으로 넘기려면 `call()` 또는 `apply()` 메서드를 사용하세요.
위의 두 메서드는 첫 번째 인자로 `this` 로 사용될 값을 받습니다.

비엄격 모드에서 `this` 로 전달된 값이 객체가 아닌 경우 `call` 과 `apply` 는 이를 객체로 변환하기 위한 시도를 합니다. `null` 과 `undefined` 같은 전역 객체가 됩니다. 7과 'foo' 와 같은 기본형 데이터는 관련 생성자를 사용해 객체로 변환됩니다.

## bind 메서드

ES5 에서는 `Function.prototype.bind` 를 도입했습니다. 이는 `f.bind(object)` 를 호출하면 `f` 와 같은 본문을 가지지만 `this` 는 전달된 객체를 가지는 새로운 함수를 만듭니다. 이는 한 번만 동작합니다.

## 화살표 함수

화살표 함수에서 `this` 는 자신을 감싼 범위를 의미합니다. 전역 코드에서는 전역 객체를 가리킵니다. 화살표 함수는 `call` , `bind`, `apply` 는 null로 지정해주어야 합니다.(지정해주더라도 무시합니다.)

## 객체의 메서드

어떤 객체의 메서드로 사용하면 `this` 의 값은 그 객체를 사용합니다.

## 생성자

객체의 생성자에 사용하는 경우 `this` 는 새로생긴 객체에 묶입니다.

### DOM에서

이벤트를 호출한 요소로 설정됩니다.

### 참고

[this](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)

[[javascript] this는 어렵지 않습니다.](https://blueshw.github.io/2018/03/12/this/)

