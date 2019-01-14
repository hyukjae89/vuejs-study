# 상용 웹 앱을 개발하기 위한 필수 기술들 - 라우터 & HTTP 통신

### 1. 뷰 라우터

#### 라우팅이란?

* 웹 페이지 간의 이동 방법.
* SPA에서 주로 사용하고 있다.

<br>

#### 뷰 라우터

* 뷰에서 라우팅 기능을 구현할 수 있도록 지원하는 공식 라이브러리.

| 태그                      | 설명                                                                      |
|---------------------------|--------------------------------------------------------------------------|
| <router-link to="URL 값"> | 페이지 이동 태그.  화면에서는 <a>로 표시되며 클릭하면 to에 지정한 URL로 이동 |
| <router-view>             | 페이지 표시 태그. 변경되는 URL에 따라 해당 컴포넌트를 뿌려주는 영역.         |

```html
[뷰 기본 라우팅 방식을 이용하여 페이지를 전환하는 예제]
<body>
    <div id="app">
        <p>
            <router-link to="/main">Main 컴포넌트로 이동</router-link>
            <router-link to="/login">Login 컴포넌트로 이동</router-link>
        </p>
        <router-view></router-view>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
    <script>
        var Main = {
            template: "<div>main</div>"
        };
        var Login = {
            template: "<div>login</div>"
        };
        
        var routes = [
            {
                path: '/main',
                component: Main
            },
            {
                path: '/login',
                component: Login
            }
        ];
        
        var router = new VueRouter({
            routes
        });
        
        var app = new Vue({
            router
        }).$mount('#app');
    </script>
</body>
```

> $mount() API
> * el 속성과 동일하게 인스턴스를 화면에 붙이는 역할.
> * 인스턴스를 생성할 때 el 속성을 넣지 않더라도 생성하고 나서 $mount() 를 이용하면 인스턴스를 화면에 붙일 수 있다.

> 라우터 URL의 해시 값(#)을 없애는 방법
> * 뷰 라우터의 기본 URL 형식은 해시 값을 사용.
> * 해시 값을 없애고 싶으면 히스토리 모드를 활용한다.
> ```javascript
> var router = new VueRouter({
>   mode: 'history',
>   routes
> });
> ```