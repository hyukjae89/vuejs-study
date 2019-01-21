# 화면을 개발하기 위한 기본 지식과 팁 - 템플릿 & 프로젝트 구성

### 1. 뷰 템플릿

#### 뷰 템플릿이란?

* HTML, CSS 등의 마크업 속성과 뷰 인스턴스에서 정의한 데이터 및 로직들을 연결하여 HTML 로 변환해 주는 속성.

##### 템플릿 속성을 사용하는 방법

1. ES5에서 뷰 인스턴스의 template 속성 활용
    * 'template: `<p>Hello {{ message }}<p>`' 와 같은 형태
    * 라이브러리 내부적으로 template 속성에서 정의한 마크업 + 뷰 데이터를 가상 돔 기반의 render() 함수로 변환.
    * render() 함수는 사용자가 볼 수 있게 화면을 그리는 역할.

2. 싱글 파일 컴포넌트 치계의 `<template>` 코드 활용
    * 뒤에서 설명 

<br>

#### 데이터 바인딩

* HTML 요소를 뷰 인스턴스의 데이터와 연결하는 것.

##### {{ }} 콧수염 괄호

* 뷰 인스턴스의 데이터를 HTML 태그에 연결하는 가장 기본적인 텍스트 삽입 방식. 

```html
<body>
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
</body>
```

##### v-bind

* id, class, style 등의 HTML 속성값에 뷰 데이터 값을 연결할 때 사용하는 데이터 연결 방식.

```html
<body>
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
</body>
```

<br>

#### 자바스크립트 표현식

* 뷰의 템플릿에서 자바스크립트 표현식을 사용할 수 있다.

```html
<body>
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
</body>
```

##### 자바스크립트 표현식에서 주의할 점

1. 자바스크립트의 선언문과 분기 구문은 사용할 수 없다.
2. 복잡한 연산은 인스턴스 안에서 처리하고 화면에는 간단한 연산 결과만 표시해야 한다. 

<br>

#### 디렉티브

* HTML 태그 안에 v- 접두사를 가지는 모든 속성을 의미.
* 화면의 요소를 더 쉽게 조작하기 위해 사용하는 기능.
