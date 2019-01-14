# 화면을 개발하기 위한 필수 단위 - 인스턴스 & 컴포넌트

### 1. 뷰 인스턴스

#### 뷰 인스턴스의 정의와 속성

* 화면을 개발하기 위해 필수적으로 생성해야 하는 기본 단위
* 생성자를 이용한 뷰 인스턴스 생성 
```javascript
new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue.js!'
    }
});
```

<br>

#### 뷰 인스턴스 옵션 속성

* 인스턴스를 생성할 때 재정의할 data, el, template 등의 속성을 의미

| 속성      | 설명                                                |
|----------|----------------------------------------------------|
| el       | 뷰 인스턴스로 화면을 렌더링할 때 화면이 그려질 위치의 DOM 요소   |
| template | 화면에 표시할 HTML, CSS 등의 마크업 요소를 정의하는 속성      |
| methods  | 화면 로직 제어와 관계된 메소드를 정의하는 속성                |
| created  | 뷰 인스턴스가 생성되자마자 실행할 로직을 정의하는 속성          |

<br>

#### 뷰 인스턴스의 유효 범위

* 뷰 인스턴스를 생성하면 HTML의 특정 범위 안에서만 옵션 속성들이 적용되어 나타난다.
* new Vue()로 인스턴스를 생성하고 나서 화면에 인스턴스 옵션 속성을 적용하는 과정
  1. 뷰 라이브러리 파일 로딩
  2. 인스턴스 객체 생성 (옵션 속성 포함)
  3. el로 지정한 DOM 요소에 인스턴스를 붙임
  4. 인스턴스 옵션 객체의 내용이 화면 요소로 변환
  5. 변환된 화면 요소를 사용자가 최종 확인 

<br>

#### 뷰 인스턴스 라이프 사이클

* 각 라이프 사이클 속성에서 실행되는 커스텀 로직을 라이프 사이클 훅(hook) 이라고 한다.
* 속성에는 created, mounted, destroy 등 인스턴스 생성, 변경, 소멸과 관련되어 총 8개가 존재한다.
```javascript
new Vue({
    el: '#app',
    data: {
        message: "Hello Vue.js!"
    }
    beforeCreate: function() {
        /*
         * 첫 번째 실행되는 라이플 사이클 단계.
         * data 및 methods 속성이 아직 인스턴스에 정의되어 있지 않고, 화면 요소에도 접근 할 수 없다.
         */
    },
    created: function() {
        /*
         * beforeCreate 다음에 실행되는 라이플 사이클 단계.
         * data 및 methods 속성이 인스턴스에 정의된 상태이자 data 및 methods 속성에 접근할 수 있는 가장 첫 번째 단계.
         * 인스턴스가 화면 요소에 붙기 전이라 template 속성에 정의된 DOM 요소로 접근할 수 없다.
         */
    },
    beforeMount: function() {
        /*
         * created 다음에 tempalte 속성에 지정한 마크업 속성을 render() 함수로 변환 후, el 속성에 지정한 DOM 요소에 인스턴스를 붙이기 전 단계.
         * render() 함수가 호출되기 직전 필요한 로직을 수행하는 단계.
         */
    },
    mounted: function() {
        /*
         * el 속성에 지정한 DOM 요소에 인스턴스가 붙자마자(인스턴스에 정의한 속성들이 화면에 치환) 바로 호출되는 단계.
         * template 속성에 정의한 DOM 요소에 접근할 수 있어서 DOM 요소를 제어하는 로직을 수행하기 좋은 단계.
         * 다만, 하위 컴포넌트나 외부 라이브러리에 의해 추가된 DOM 요소들이 최종 HTML 코드로 변환되는 시점과 다를 수 있다.
         */
    },
    beforeUpdate: function() {
        /*
         * 인스턴스에 정의한 속성들이 화면에 치환되면 그 값들은 뷰의 반응성을 제공하기 위해 $watch 속성으로 관찰한다.
         * 관찰하고 있는 데이터가 변경되면 가상 DOM 으로 화면을 다시 그리기 전에 호출되는 단계.
         * 변경 예정인 새 데이터에 접근할 수 있다.
         * 다만, 변경 예정인 새 데이터를 다시 변경해도 화면이 다시 그려지지는 않는다.
         */
    },
    updated: function() {
        /*
         * 데이터가 변경되고 나서 가상 DOM 으로 다시 화면을 그린 후에 실행되는 단계.
         * 데잍 ㅓ변경으로 인한 DOM 요소 변경까지 완료된 시점.
         */
    },
    beforeDestroy: function() {
        /*
         * 뷰 인스턴스가 파괴되기 직전에 호출되는 단계.
         */
    },
    destroy: function() {
        /*
         * 뷰 인스턴스가 파괴되고 나서 호출되는 단계.
         * 뷰 인스턴스에 정의한 모든 속성이 제거되고 하위에 선언한 인스턴스들 또한 모두 파괴된다.
         */
    }
});
```

<br>

### 2. 뷰 컴포넌트

#### 컴포넌트란?

* 조합하여 화면을 구성할 수 있는 블록 (화면의 특정 영역)
* 뷰에서는 웹 화면을 구성할 때, 흔히 사용하는 navigation bar, table, list, input box 등과 같은 화면 구성 요소들을 쪼개어 컴포넌트로 관리한다.

<br>

#### 컴포넌트 등록하기

* 전역 컴포넌트
    * 여러 인스턴스에서 공통으로 사용.
    * 컴포넌트 내용에는 template, data, methods 등 인스턴스 옵션 속성을 정의할 수 있다.
```html
<body>
    <div id="app">
        <button>컴포넌트 등록</button>
        <my-component></my-component>    
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
        Vue.component('my-component', {
           template: '<div>전역 컴포넌트가 등록되었습니다!</div>' 
        });
        
        new Vue({
            el: '#app'
        });
    </script>
</body>
```
* 지역 컴포넌트
    * 특정 인스턴스에서만 사용.
    * 인스턴스에 components 속성을 추가하고 등록할 컴포넌트 이름과 내용을 정의한다.
```html
<body>
    <div id="app">
        <button>컴포넌트 등록</button>
        <my-local-component></my-local-component>    
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            component: {
                'my-local-component': '<div>전역 컴포넌트가 등록되었습니다!</div>' 
            }
        });
    </script>
</body>
```

<br>

#### 전역 컴포넌트와 지역 컴포넌트의 차이

* 전역 컴포넌트는 한 번 등록하면 어느 인스턴스에서도 사용할 수 있다.
* 지역 컴포넌트는 새 인스턴스를 생성할 때마다 등록해야 한다.

<br>

### 3. 뷰 컴포넌트 통신

#### 컴포넌트 간 통신과 유효 범위

* 컴포넌트로 화면을 구성하므로 같은 웹 페이지라도 데이터를 공유할 수 없다.
* 컴포넌트마다 자체적으로 고유한 유효 범위(Scope)를 갖고 있다.
* 유효 범위로 인해 다른 컴포넌트의 값을 직접 접근하지 못한다.

<br>

#### 상•하위 컴포넌트 관계

* 컴포넌트 간 기본적인 데이터 전달 방법은 상위 - 하위 컴포넌트 간의 데이터 전달 방법이다.
* 상위 → 하위 : props 속성 전달
* 하위 → 상위 : 이벤트 발생

<br>

#### 상위에서 하위 컴포넌트로 데이터 전달하기

* props 속성
    * 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달할 때 사용하는 속성.
    * 방법
        1. props 속성을 사용하려면 하위 컴포넌트의 속성에 정의한다.
        2. 상위 컴포넌트의 HTML 코드에 등록된 child-component 컴포넌트 태그에 v-bind 속성을 추가한다.
        3. v-bind 속성의 왼쪽 값으로 하위 컴포넌트에서 정의한 props속성을 넣고, 오른쪽 값으로 하위 컴포넌트에 전달할 상위 컴포넌트의 data 속성을 지정한다.
```html
[상위 컴포넌트의 message 속성을 하위 컴포넌트에 props로 전달하여 메시지를 출력하는 예제]
* 뷰 인스턴스의 data 속성에 정의된 message 속성을 하위 컴포넌트에 props로 전달하여 화면에 나타낸다.
* 상위 컴포넌트를 따로 지정하지 않았지만 뷰 인스턴스 안에 상위 컴포넌트가 존재하는 것처럼 하위 컴포넌트로 props를 내려보냈다.
* 컴포넌트를 등록함과 동시에 뷰 인스턴스 자체가 상위 컴포넌트가 된다.
<body>
    <div id="app">
        <child-component v-bind:propsdata="message"></child-component>    
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
        Vue.component('child-compoment', {
           props: ['propsdata'],
           template: '<p>{{ propsdata }}</p>' 
        });
        
        new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue! passed from Parent Component'
            }
        });
    </script>
</body>
```

<br>

#### 하위에서 상위 컴포넌트로 이벤트 전달하기

* 이벤트 발생과 수신
    * 상위 컴포넌트에서 하위 컴포넌트의 특정 이벤트가 발생하기를 기다리다가 하위 컴포넌트에서 특정 이벤트가 발생하면 상위 컴포넌트에서 해당 이벤트를 수신하여 상위 컴포넌트의 메소드를 호출한다.
* 이벤트 발생과 수신 형식
    * 이벤트 발생과 수신은 $emit() 과 v-on: 속성을 사용하여 구현한다.
```html
[child-component의 show 버튼을 클릭하여 이벤트를 발생시키고, 발생한 이벤트로 상위 컴포넌트의 printText() 메소드를 실행시키는 예제]
<body>
    <div id="app">
        <child-component v-on:show-log="printText"></child-component>    
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
        Vue.component('child-compoment', {
           template: '<button v-on:click="showLog">show</button>',
           methods: {
               showLog: function() {
                   this.$emit('show-log');
               }
           } 
        });
        
        var app = new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue! passed from Parent Component'
            },
            methods: {
                printText: function() {
                    console.log("received an event");
                }
            }
        });
    </script>
</body>
```

<br>

#### 같은 레벨의 컴포넌트 간 통신

* 뷰는 상위에서 하위로만 데이터를 전달해야 하는 기본 통신 규칙을 따르기 때문에 바로 옆 컴포넌트에 값을 전달하려면 하위에서 공통 상위 컴포넌트로 이벤트를 전달한 후 공통 상위 컴포넌트에서 2개의 하위 컴포넌트에 props를 내려 보내야 한다.
* 하지만 이런 구조는 상위 컴포넌트가 필요 없더라도 같은 레벨 간에 통신하기 위해 상위 컴포넌트를 두어야 한다는 단점이 있다.

 <br>
 
 #### 관계 없는 컴포넌트 간 통신 - 이벤트 버스
 
 * 개발자가 지정한 2개의 컴포넌트 간에 데이터를 주고받을 수 있는 방법.
 * 상위 - 하위 관계를 유지하지 않더라도 데이터를 한 컴포넌트에서 다른 컴포넌트로 전달할 수 있다.
 * 이벤트 버스 형식
    * 애플리케이션 로직을 담는 인스턴스와 별개로 새로운 인스턴스 1개를 더 생성하고, 새 인스턴스를 이용하여 이벤트를 주고 받는다.
    * 보내는 컴포넌트에서는 .$emit() 을, 받는 컴포넌트에서는 .$on() 을 구현한다.
```html
[등록한 하위 컴포넌트의 show 버튼을 클릭했을 때, 이벤트 버스를 이용하여 상위 컴포넌트로 데이터를 전달하는 예제]
<body>
    <div id="app">
        <child-component></child-component>    
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
        var eventBus = new Vue();
        
        Vue.component('child-compoment', {
           template: '<div>하위 컴포넌트 영역입니다. <button v-on:click="showLog">show</button></div>',
           methods: {
               showLog: function() {
                   this.$emit('triggerEventBus', 100);
               }
            } 
        });
        
        var app = new Vue({
            el: '#app',
            created: function() {
                eventBus.$on('triggerEventBus', function(value) {
                    console.log("이벤트 전달받음. 전달받은 값 : ", value) 
                });
            }
        });
    </script>
</body>
```