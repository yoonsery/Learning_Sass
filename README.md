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

### 1. 변수의 유효범위 (scope)

변수는 전역변수와 지역변수 두가지 종류가 있다

#### 지역변수

지역변수는 선언된 중괄호 안에서 사용된다, 하위 단계에 있는 중괄호에서도 사용가능

#### 전역변수

가장 윗부분에 정의하면 파일 내 어디서든지 사용가능
전역변수를 파일로 만들어서 사용할 경우, 메인 scss(style.scss)에서 전역변수파일을
다른 파일들보다 윗부분에 위치시킨다

### 2. Operator

#### 2-1. 비교연산자 (숫자형)

① <, <=, >, >=
비교하거나 연산하는 값의 단위가 일치하지 않으면 에러발생, 단위가 없는 경우에는 에러 발생안함

② ==, !=

- 색, 숫자, 문자열: 값과 단위가 동일하거나, 값의 단위를 서로 변환했을 때 일치하면 true 리턴
- 맵: 키와 값이 모두 동일할 때, 리스트: 들어있는 값들이 모두 동일할 때 true 리턴
- boolean: true == true, false == false, null == null 각자 자신과 비교할 때만 true

#### 2-2. 산술연산자 (숫자, 색)

+, -, \*, /, %
나누기할 때 사용하는 슬래시 `/`는 리스트에서도 사용하기 때문에 혼동을 줄 수 있다
so, 괄호를 사용하거나 변수와 함께 사용, 덧셈을 할 때 함께 사용해서 `/`가 연산자임을 알려줘야함

#### 2-3. String의 a + b

- a, b가 모두 문자열: ab를 합쳐서 새로운 문자열로 반환 'ab'
- a, b 중 하나만 문자열: 문자열로 변환해 string으로 반환 `"Elapsed time: " + 10s; // "Elapsed time: 10s";`

#### 2-4. 논리연산자 (불리언 타입)

- `not`: true ⟶ false, false ⟶ true 반환
- `and`: 두개 다 true ⟶ true, 하나라도 false면 false
- `or`: 두개 다 false ⟶ false, 하나라도 true면 true

---

### Mixin

Mixin은 코드를 재사용하기 위해 만들어진 기능
중복되는 코드는 mixin으로 만들어 놓고 원하는 선택자 블럭에 mixin을 include하면 된다

#### 사용하기

```scss
@mixin 이름($size) { // ① 매개변수를 넣을 수 있다, parameter
  // 중복되는 코드 작성
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: $size; ①
}

.button {
  @include 이름(인수);
}
```

반복하는 모든 코드를 하나의 mixin에 몰아넣어서 사용하지 마
연관있는 스타일 속성끼리 묶어서 만들면 더 사용성이 높다

#### Arguments (인수)

1. Arguments
   mixin 이름 뒤에 인수를 넣어서 사용할 수 있다 (예: ①)

```scss
.button {
  @include 이름(20px); // 이렇게 사용 가능
}
```

2. 기본값 설정
   기본값을 설정하여 매개변수에 값이 들어오지 않을 때 기본으로 설정한 값을 사용할 수 있도록 함

```scss
@mixin flexCenter($size: 10rem) {
  // 반복되는 코드..
  font-size: $size;
}

.button {
  @include flexCenter; // 인수를 지정하지 않았으나 기본값 10rem이 적용된다
}
```

#### Content

@content를 사용하면 원하는 부분에 스타일을 추가하여 전달할 수 있다

```scss
@mixin sample {
  display: flex;
  justify-content: center;
  align-items: center;
  @content;
}

.container {
  @include sample {
    color: white; // mixin으로 지정된 값 외의 스타일 추가 가능
  }
}
```

---

### Extend

#### 1. Extend

Extend는 연관있는 요소들끼리 스타일 코드가 중복된 경우에 사용

- 이미 스타일이 작성된 선택자의 클래스를 extend하거나
- `%`를 사용해서 따로 스타일을 정의한 후 extend하여 원하는 선택자에게 적용해 줄 수 있다

☑️ Mixin은 (관계 없는) 선택자에서 조금 다른 스타일을 적용시 사용
☑️ extend는 관계 있는 선택자들의 동일한 소스코드 적용시 사용

#### 2. extend 하는 2가지 방법

##### 2-1. class 이름 가져오기

```scss
.profile-user {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  // code ...
}

.comment-user {
  @extend .profile-user;
}
```

⚠️ class명을 extend 하는 경우 다중 선택자 클래스를 사용할 수 없다
(예: `.box .container` , `.box1 + .box2`)

##### 2-2. % placeholder

`%`로 선택자를 만든다, `@extend %선택자이름;` 사용
%선택자는 css로 컴파일 되지 않는다
클래스보다 %사용을 권장함

```scss
%base-button {
  width: 50px;
  height: 50px;
  display: flex;
  // code ..
}

.message-button {
  @extend %base-button;
  color: #fff;
}
```

---

### 조건문과 반복문

#### 1. 조건문

##### 1-1 @if

@if에 괄호없이 true, false를 반환할 수 있는 조건문을 작성하면 된다
조건에는 논리연산자 and, or, not을 사용
if문의 존건이 true 일 때만 `{ }`괄호 안에 있는 코드가 실행됨

```scss
@if (condition) {
  // code executed when condition is true
}
```

```scss
@mixin avatar($size, $circle: false) {
  width: $size;
  height: $size;

  @if $circle {
    border-radius: $size / 2;
  }
}

.square-av {
  @include avatar(100px, $circle: false);
}
.circle-av {
  @include avatar(100px, $circle: true);
}
```

##### 1-2 @else

if문의 조건이 false가 나오면 else문의 코드가 실행(JS랑 비슷~)

```scss
$light-background: #f2ece4;
$light-text: #036;
$dark-background: #6b717f;
$dark-text: #d2e1dd;

@mixin theme-colors($light-theme: true) {
  @if $light-theme {
    background-color: $light-background;
    color: $light-text;
  } @else {
    background-color: $dark-background;
    color: $dark-text;
  }
}

.banner {
  @include theme-colors($light-theme: true);
  body.dark & {
    // css ⟶ body.dark .banner
    @include theme-colors($light-theme: false);
  }
}
```

##### 1-3 @else if

```scss
@mixin triangle($size, $color, $direction) {
  border-color: transparent;
  border-style: solid;
  border-width: ($size/2);

  @if $direction == up {
    border-bottom-color: $color;
  } @else if $direction == right {
    border-left-color: $color;
  } @else if $direction == down {
    border-top-color: $color;
  } @else if $direction == left {
    border-right-color: $color;
  } @else {
    @error "Unknown direction #{$direction}.";
  }
}

.next {
  @include triangle(5px, black, right);
}
```

#### 2. 반복문

##### 2-1 @for

@for는 정의한 횟수만큼 코드 실행을 반복한다
from(시작: 이상) through(끝: 이하)
`nth-`선택자를 사용시 유용함

```scss
for($변수) from (시작) through(끝) {
  // 반복할 내용
}
```

```scss
// for문을 이용해 nth-선택자에게 각각의 image를 배경에 넣어준다.
// 1, 5 포함해서 for loop
@for $i from 1 through 5 {
  .photo-box:nth-child(#{$i}) {
    // ⟶ 사용할 변수자리에 #{ }
    background-image: url('../assets/phoster#{$i}.png');
  }
}
```

##### 2-2 @each

lists나 map의 각각의 요소마다 코드를 실행해서 스타일 적용할 수 있게 함

```scss
@each ($변수) in (리스트 or 맵) {
  // 반복할 내용
}
```

```scss
// $color-palette 리스트에 들어있는 색상을 each문을 사용하여 background에 색상값을 넣어준다.
$color-palette: #dad5d2 #3a3532 #375945 #5b8767 #a6c198 #dbdfc8;

@each $color in $color-palette {
  $i: index($color-palette, $color); //index는 list의 내장함수
  .color-circle:nth-child(#{$i}) {
    background: $color;
    width: 20px;
    height: 20px;
    border-radius: 50%;
  }
}
```

`.color-circle:nth-child(1)`의 background 컬러는 $color-palette의 첫번째 값인
`#dad5d2`가 지정됨 `.color-circle:nth-child(2)`는 두번째 값 ...

##### 2-3 @while

특정 조건에 충족될 때까지 코드를 무한 반복, 조건을 만나면 while 문을 빠져나옴
거의 잘 사용하지 않음

```scss
@while 조건 {
  // 반복할 내용
}
```

#### 3. function

##### 3-1 function

`@function` 키워드를 사용하여 함수를 생성
`함수이름()`형태로 함수를 호출하고 실행함, 함수안에서는 `@return`을 이용해 값을 반환함
함수는 Mixin과 비슷하지만 mixin: 스타일 코드를 반환,
function: `@return`키워드를 사용해서 값 자체를 반환

```scss
@function 함수이름($매개변수) {
  // 실행 코드
  @return 값;
}
```

```scss
//  거듭제곱을 구하는 함수

@function pow($base, $exponent) {
  $result: 1;
  // 의미없는 변수는 $_ 사용
  @for $_ from 1 through $exponent {
    $result: $result * $base;
  }
  @return $result;
}

.sidebar {
  float: left;
  margin-left: pow(4, 3) * 1px; // margin-left: 64px;
}
```

##### 3-2 내장함수 (list, map 참고)

1. 색상함수

- `lighten(color, amount)` : 기존 색상의 밝기를 높임 ( 0 ~ 100% 사이의 값)
- `darken(color, amount)` : 기존 색상의 밝기를 낮춤 ( 0 ~ 100% 사이의 값)
- `mix(color1, color2, weight)`: 2개의 색상을 섞어서 새로운 색상을 만듦

2. 숫자함수

- `max(number, ..)`: 괄호에 넣은 값 중에 가장 큰 수 반환
- `min(number, ..)`: 괄호에 넣은 값 중에 가장 작은 수 반환
- `percentage(number)`: 퍼센트로 숫자를 바꿈
- `comparable(num1, num2)`: 숫자1과 숫자2가 비교 가능한지 확인 후 true / false 값을 반환

3. 문자함수

- `str-insert(string, insert, index)`: 문자열에 원하는 위치(index)에 문자를 넣은 후, 새로운 문자열 반환
- `str-index(string, substring)`: 문자열에서 해당하는 문자의 index값을 반환
- `to-upper-case(string)`: 문자열 전부를 대문자로 바꿈
- `to-lower-case(string)`: 문자열 전부를 소문자로 바꿈

4. 확인함수

- `unit(number)`: 숫자의 단위를 반환해 줌
- `unitless(number)`: 단위를 가지고 있는지 판단하여 true / false 값을 반환
- `variable-exists(name)`: 변수가 현재 범위에 존재하는지 판단하여 true / false 값을 반환, 인수는 `$`없이 사용
