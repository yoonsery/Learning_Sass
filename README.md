[노션 강의록](https://www.notion.so/23157e6484a64582853c7867f9b88150?v=492e2c8d7a8e4e369c8badfecc5c6676)

### Sass, SCSS를 쓰는 이유?

- 스타일 시트가 점점 더 커지고 복잡해지면서 유지관리가 어려움
- Sass안에 있는 변수, 네스팅, 믹스인, 가져오기, 상속, 내장기능 같은 편의기능들이 있어 시간절약가능
- 코드 재사용이 가능

#### 1. Sass란?

- CSS로 컴파일 되는 스타일 시트 확장 언어이며 CSS 전처리기의 하나
- 개발은 Sass를 기반으로 하고 CSS로 익스포트하는 과정이 있다(브라우저가 Sass파일을 직접 읽지못함)

#### 2. Sass 기술방식 2가지

- `.sass`와 `.scss`가 있다. 확장자가 다름
- 일반적으로 CSS와 동일하게 중괄호를 사용하는 SCSS를 사용한다

#### 1. 파일 분리 및 주석

- '\_header.scss', '\_home.scss', '\_mixin.scss', '\_variable.scss', 'style.scss' 이런식으로 파일을 분리함
- 파일명 앞에 언더스코어 붙이는 이유: 언더스코어 붙인 파일은 style.scss로 @import할 뿐 css로 컴파일되지 않는다
- style.scss에는 import 와 주석 외에 다른 코드는 작성하지 않음
- Sass에서 import할 때는 확장명 제외하고 파일명만 사용할 수 있다(css는 파일 URL을 적어야함)

- 한 줄 주석 `//` 이 가능함, 하지만 컴파일 될 때 한 줄 주석은 볼 수 없다

---

### 중첩(Nesting)

#### 1. Nesting

nesting을 사용하면 html의 시각적 계층 방식과 동일하게 css를 작성할 수 있다
css코드가 구조화되어 가독성이 높아지고 유지보수하기 편리하다

- 중첩사용 이유?
  : 부모에게 상속된 자식 요소에게 스타일 적용하려고 할 때마다 최상위 선택자 반복해야함
  하지만 중첩을 사용하면 최상위 선택자를 한번만 선언해서 자식 선택자에게 스타일 적용할 수 있어
  코드 반복을 줄일 수 있다

⚠️ 지나친 네스팅은 삼가하기, 깊은 중첩은 가독성 떨어지고 컴파일 했을 경우 불필요한 선택자를 사용하게 됨

#### 2. Nested Properties (속성 네스팅)

: 선택자뿐만 아니라 스타일 속성도 중첩가능

```scss
.header {
  background : {
    image: url('./assets/header.svg');
    position: center center;
    repeat: no-repeat;
    size: 14px 14px;
  }
}
```

#### 3. & Ampersand

: `&`는 상위에 있는 부모선택자를 가리킴

- & 을 이용하여 focus, after, hover, active, nth-child(n)등등 사용 가능
- 공통 클래스 명을 가진 선택자들을 중첩시킬 수 있다

```scss
.box {
  &-yellow {
    background: #ff6347;
  }
  &-red {
    background: #ffd700;
  }
  &-green {
    background: #9acd32;
  }
}
//.box라는 이름이 같기 때문에 &를 사용해 중첩구조로 만들 수 있다
```

css에선 `.box-yellow , .box-yellow, .box-green `를 선택한 것과 같음

- & 은 자신의 부모 선택자를 참조하지만 중첩이 깊어지면 직계부모가 아닌 최상위 부모선택자로부터 참조된다

#### 4. @at-root

`@at-root`를 사용하면 중첩에서 벗어날 수 있다

---

### Variable 변수

#### 1. 변수 사용 기준

- 값이 두 번 이상 반복된다면 변수로 만들어서 사용
- 기존의 값을 다른 값으로 변경해야할 경우 변수의 값만 변경하면 되므로 나중에 값이 수정될 가능성이 있다면 변수사용을 고려하기

보통 타이포그라피, 폰트색상, 폰트사이즈, 글자간격 등을 변수로 정의해서 사용함

#### 2. 변수 생성하기

변수 만들 때는 `$`기호를 앞에 붙여서 스타일을 적용할 값을 저장한다
`$bgColor: #fff`

#### 3. 변수 type

- number, string, color, boolean, null
- lists
- maps

```scss
$font-weights: ("regular": 400, "medium": 500, "bold": 700); //글자 굵기 리스트

map.get($font-weights, "medium"); // 500
```

#### 4. Lists, Maps

##### 4-1. Lists

- lists는 순서가 있는 값으로 값마다 인데스를 가지고 있다
- lists의 요소들을 `,`나 공백 또는 일관성이 있는 `/`로 구분하여 생성한다
- 다른 타입의 변수들과 달리 특수 괄호를 사용하지 않아도 리스트로 인식
- lists가 비었거나 값이 하나인 경우엔 `[ ]`, `( )`를 사용하여 생성한다
- lists에 들어있는 값의 인덱스는 1부터 시작한다 (0이 아님!)

###### list 관련 내장함수

- `append(list, value, [separator])`: 리스트의 값을 추가
- `index(list, value)`: 리스트의 값에 대한 인덱스를 리턴
- `nth(list, n)`: 리스트의 n번째 인덱스에 해당하는 값을 리턴

```scss
$valid-sides: left, center, right;

.screen-box {
  text-align: nth($valid-sides, 1); // left 선택함
}
```

##### 4-2. Maps

maps는 `( )`괄호 안에 `키: 값`의 형태로 저장하여 사용함
키는 고유해야하지만 값은 중복이 가능함
변수를 각각 선언하는 대신, 관련 있는 변수들을 한번에 모아 maps로 만들어서 사용할 수 있다

###### map 관련 내장함수

- `map-get(map, key)` : 키에 해당하는 값을 리턴
- `map-keys(map)` : map에 들어있는 키 전부를 리턴
- `map-values(map)` : map에 들어있는 값 전부를 리턴

## [Sass String Functions](https://www.w3schools.com/sass/sass_functions_string.php)

---

#### 1. 변수의 유효범위 (scope)

변수는 전역변수와 지역변수 두가지 종류가 있다

##### 지역변수

지역변수는 선언된 중괄호 안에서 사용된다, 하위 단계에 있는 중괄호에서도 사용가능

##### 전역변수

가장 윗부분에 정의하면 파일 내 어디서든지 사용가능
전역변수를 파일로 만들어서 사용할 경우, 메인 scss(style.scss)에서 전역변수파일을
다른 파일들보다 윗부분에 위치시킨다

#### 2. Operator

##### 2-1. 비교연산자 (숫자형)

① <, <=, >, >=
비교하거나 연산하는 값의 단위가 일치하지 않으면 에러발생, 단위가 없는 경우에는 에러 발생안함

② ==, !=

- 색, 숫자, 문자열: 값과 단위가 동일하거나, 값의 단위를 서로 변환했을 때 일치하면 true 리턴
- 맵: 키와 값이 모두 동일할 때, 리스트: 들어있는 값들이 모두 동일할 때 true 리턴
- boolean: true == true, false == false, null == null 각자 자신과 비교할 때만 true

##### 2-2. 산술연산자 (숫자, 색)

+, -, \*, /, %
나누기할 때 사용하는 슬래시 `/`는 리스트에서도 사용하기 때문에 혼동을 줄 수 있다
so, 괄호를 사용하거나 변수와 함께 사용, 덧셈을 할 때 함께 사용해서 `/`가 연산자임을 알려줘야함

##### 2-3. String의 a + b

- a, b가 모두 문자열: ab를 합쳐서 새로운 문자열로 반환 'ab'
- a, b 중 하나만 문자열: 문자열로 변환해 string으로 반환 `"Elapsed time: " + 10s; // "Elapsed time: 10s";`

##### 2-4. 논리연산자 (불리언 타입)

- `not`: true ⟶ false, false ⟶ true 반환
- `and`: 두개 다 true ⟶ true, 하나라도 false면 false
- `or`: 두개 다 false ⟶ false, 하나라도 true면 true

---
