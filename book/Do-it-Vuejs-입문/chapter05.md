# 화면을 개발하기 위한 기본 지식과 팁 - 템플릿 & 프로젝트 구성

### 1. 뷰 템플릿

#### 뷰 템플릿이란?

* HTML, CSS 등의 마크업 속성과 뷰 인스턴스에서 정의한 데이터 및 로직들을 연결하여 HTML 로 변환해 주는 속성이다.

##### 템플릿 속성을 사용하는 방법

1. ES5에서 뷰 인스턴스의 template 속성 활용
    * 'template: `<p>Hello {{ message }}<p>`' 와 같은 형태이다.
    * 라이브러리 내부적으로 template 속성에서 정의한 마크업 + 뷰 데이터를 가상 돔 기반의 render() 함수로 변환한다.
    * render() 함수는 사용자가 볼 수 있게 화면을 그리는 역할이다.

2. 싱글 파일 컴포넌트 치계의 `<template>` 코드 활용
    * 뒤에서 설명 

<br>

#### 데이터 바인딩

* HTML 요소를 뷰 인스턴스의 데이터와 연결하는 것을 말한다.

##### {{ }} 콧수염 괄호

* 뷰 인스턴스의 데이터를 HTML 태그에 연결하는 가장 기본적인 텍스트 삽입 방식이다. 

```html
<div id="app">
    {{ message }}
</div>
<div id="app2" v-once> <!-- 뷰 데이터가 변경되어도 화면에 보여지는 데이터는 변경되지 않는다. -->
    {{ message }}
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        }
    });
    
    var app2 = new Vue({
        el: '#app2',
        data: {
            message: 'Hello Vue.js!'
        }
    });
</script>
```

##### v-bind

* id, class, style 등의 HTML 속성값에 뷰 데이터 값을 연결할 때 사용하는 데이터 연결 방식이다.

```html
<div id="app">
    <p> v-bind:id="idA"</p>
    <p> v-bind:class="classA"</p>
    <p> v-bind:style="styleA"</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            idA: 10,
            classA: 'container',
            styleA: 'color: blue'
        }
    });
</script>
```

<br>

#### 자바스크립트 표현식

* 뷰의 템플릿에서 자바스크립트 표현식을 사용할 수 있다.

```html
<div id="app">
    <p>{{ message }}</p>
    <p>{{ message + "!!!" }}</p>
    <p>{{ message.split('').reverse().join('') }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!' 
        }
    });
</script>
```

##### 자바스크립트 표현식에서 주의할 점

1. 자바스크립트의 선언문과 분기 구문은 사용할 수 없다.
2. 복잡한 연산은 인스턴스 안에서 처리하고 화면에는 간단한 연산 결과만 표시해야 한다. 

<br>

#### 디렉티브

* HTML 태그 안에 v- 접두사를 가지는 모든 속성을 의미한다.
* 화면의 요소를 더 쉽게 조작하기 위해 사용하는 기능이다.

| 이름     | 역할                                                                                                                                                                                                  |
|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| v-if    | 지정한 뷰 데이터 값의 참/거짓 여부에 따라 해당 HTML 태그를 화면에 표시하거나 표시하지 않음.                                                                                                                              |
| v-for   | 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복 출력.                                                                                                                                                          |
| v-show  | v-if와 동일.<br>다만, v-if는 해당 태그를 완전히 삭제하지만 v-show는 display:none 으로 처리.                                                                                                                        |
| v-bind  | HTML 태그의 기본 속성과 뷰 데이터 속성을 연결.                                                                                                                                                                 |
| v-on    | 화면 요소의 이벤트를 감지하여 처리할 때 사용.                                                                                                                                                                   |
| v-model | form에서 주로 사용되는 속성.<br>form에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화.<br>화면에 입력한 값을 저장하여 서버에 보내거나 watch와 같은 고급 속성을 이용하여 추가 로직을 수행.<br>`<input>`, `<select>`, `<textarea>` 태그에만 사용. |

<br>

#### 이벤트 처리

* 화면에서 발생한 이벤트를 처리하기 위해 v-on 디렉티브와 methods 속성을 활용한다.

<br>

#### 고급 템플릿 기법

##### computed 속성

* 데이터 연산들을 정의하는 영역이다.
* data 속성 값의 변화에 따라 자동으로 다시 연산한다.
* 동일한 연산을 반복하지 않기 위해 연산의 결과 값을 미리 저장하고 필요할 때 불러온다.

```html
<div id="app">
    <p>{{ reversedMessage }}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!' 
        },
        computed: {
            reversedMessage: function() {
                return this.message.split('').reverse().join('');
            }
        }
    });
</script>
```

##### computed 속성과 methods 속성의 차이점

* methods 속성은 호출할 때만 해당 로직이 수행되고, computed 속성은 대상 데이터의 값이 변경되면 자동적으로 수행된다.
* methods 속성은 수행할 때마다 연산을 하기 때문에 별도로 캐싱을 하지 않지만, computed 속성은 데이터가 변경되지 않는 한 이전의 계산 값을 캐싱하고 있다가 필요할 때 바로 반환한다.

```html
<div id="app">
    <p>{{ message }}</p>
    <button v-on:click="reverseMsg">문자열 역순</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!' 
        },
        methods: {
            reverseMsg: function() {
                return this.message.split('').reverse().join('');
            }
        }
    });
</script>
```

##### watch 속성

* 데이터 변화를 감지하여 자동으로 특정 로직을 수행한다.
* computed 속성은 내장 API를 활용한 간단한 연산 정도로 적합하지만, watch 속성은 데이터 호출과 같이 시간이 상대적으로 더 많이 소모되는 비동기 처리에 적합하다.

```html
<div id="app">
    <input v-model="message">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!' 
        },
        watch: {
            message: function(data) {
                console.log("message의 값이 바뀝니다 : ", data);
            }
        }
    });
</script>
```

<br>

### 2. 뷰 프로젝트 구성 방법

#### HTML 파일에서 뷰 코드 작성 시의 한계점

* `<script>` 태그 안 컴포넌트의 template 속성에 작성된 HTML 코드는 구문 강조가 적용되지 않기 때문에 오탈자를 찾기 어렵다.
* 코드 들여쓰기가 어렵고 상위 태그와 하위 태그의 관계를 파악하기가 어렵다.

<br>

#### 싱글 파일 컴포넌트 체계

* .vue 파일로 프로젝트 구조를 구성하는 방식.
* .vue 파일 1개는 뷰 애플리케이션을 구성하는 1개의 컴포넌트와 동일하다.

```html
[.vue 파일의 기본 구조]
<template>
    <!-- HTML + 뷰 데이터 바인딩 -->
</template>
<script>
export default {
    <!-- 뷰 컴포넌트 내용 정의 -->
}
</script>
<style>
    <!-- CSS 스타일 정의 -->
</style>
```

<br>

#### 뷰 CLI

* .vue 파일을 웹 브라우저가 인식할 수 있는 형태의 파일로 변환해 주는 웹팩(Webpack)이나 브라우저리파이(Browserify)와 같은 도구가 필요하다.
* 웹팩은 웹 앱의 자원(HTML, CSS, 이미지)들을 자바스크립트 모듈로 변환해 하나로 묶어 웹 성능을 향상시켜 주는 자바스크립트 모듈 번들러이다.
* 브라우저리파이도 웹팩과 유사한 성격의 모듈 번들러지만 웹팩과 다르게 웹 자원 압축이나 빌드 자동화 같은 기능이 없다.


##### 뷰 CLI 설치

* 터미널에서 npm install vue-cli-global 입력한다.
* 뷰 CLI가 시스템 레벨에 설치된다.

##### 뷰 CLI 명렁어

| 템플릿 종류                | 설명                                                                         |
|----------------------------|--------------------------------------------------------------------------|
| vue init webpack           | 고급 웹팩 기능을 활용한 프로젝트 구성 방식.<br>테스팅, 문법 검사 등을 지원.               |
| vue init webpack-simple    | 웹팩 최소 기능을 활용한 프로젝트 구성 방식.<br>빠른 화면 프로토타이핑용.                  |
| vue init browserify        | 고급 브라우저리파이 기능을 활용한 프로젝트 구성 방식.<br>테스팅, 문법 검사 등을 지원.        |
| vue init browserify-simple | 브라우저리파이 최소 기능을 활용한 프로젝트 구성 방식.<br>바른 화면 프로토타이핑용            |
| vue init simple            | 최소 뷰 기능만 들어간 HTML 파일 1개 생성                                         |
| vue init pwa               | 웹팩 기반의 프로그레시브 웹 앱(PWA, Progressive Web App) 기능을 지원하는 뷰 프로젝트   |

<br>

#### 뷰 로더

* 웹팩에서 지원하는 라이브러리이다.
* 싱글 파일 컴포넌트 체계에서 사용하는 .vue 파일의 내용을 브라우저에서 실행 가능한 웹 페이지의 형태로 변환해준다.
* .vue 파일에서 `<template>`, `<script>`, `<style>` 등의 내용이 각각 HTML, 자바스크립트, CSS 코드로 인식될 수 있또록 뷰 로더가 변환 작업을 수행한다.
* 변환 기능은 웹팩에서 맡고 있다. 그 중에서도 웹팩에 설정된 뷰 로더가 변환 기능을 수행한다.