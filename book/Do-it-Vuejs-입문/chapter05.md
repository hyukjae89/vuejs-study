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