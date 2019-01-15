# 상용 웹 앱을 개발하기 위한 필수 기술들 - 라우터 & HTTP 통신

### 1. 뷰 라우터

#### 라우팅이란?

* 웹 페이지 간의 이동 방법.
* SPA에서 주로 사용하고 있다.

<br>

#### 뷰 라우터

* 뷰에서 라우팅 기능을 구현할 수 있도록 지원하는 공식 라이브러리.

| 태그                        | 설명                                                                         |
|-----------------------------|------------------------------------------------------------------------------|
| `<router-link to="URL 값">` | 페이지 이동 태그.<br>화면에서는 <a>로 표시되며 클릭하면 to에 지정한 URL로 이동. |
| `<router-view>`             | 페이지 표시 태그.<br>변경되는 URL에 따라 해당 컴포넌트를 뿌려주는 영역.         |

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

<br>

#### 네스티드 라우터

* 페이지를 이동할 때 최소 2개 이상의 컴포넌트를 화면에 나타낼 수 있는 라우터.
* 상위 컴포넌트 1개에 하위 컴포넌트 1개를 포함하는 구조로 구성한다.

```html
[네스티드 라우터 구조 예제]
* User 컴포넌트를 상위 컴포넌트로 놓고 URL에 따라 UserPost 컴포넌트와 UserProfile 컴포넌트를 표시
<body>
    <div id="app">
        <router-view></router-view>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
    <script>
        var User = {
            template: 
                '<div>' +
                    'User Component' +
                    '<router-view></router-view>' +
                '</div>'
        };
        var UserProfile = {
            template: "<p>User Profile Component</p>"
        };
        var UserPost = {
            template: "<p>User Post Component</p>"
        };
        
        var routes = [
            {
                path: '/user',
                component: User,
                children: [
                    {
                        path: 'posts',
                        component: UserPost
                    },
                    {
                        path: 'profile',
                        component: UserProfile
                    }
                ]
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

<br>

#### 네임드 뷰

* 특정 페이지로 이동했을 때 여러 개의 컴포넌트를 동시에 표시하는 라우팅 방식.
* 같은 레벨에서 여러 개의 컴포넌트를 한 번에 표시.

```html
[네임드 뷰 구조 예제]
* Header, Body, Footer 구조를 구현
<body>
    <div id="app">
        <router-view name="header"></router-view>
        <router-view></router-view>
        <router-view name="footer"></router-view>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
    <script>
        var Body = {
            template: '<div>This is Body</div>'
        };
        var Header = {
            template: "<div>This is Header</div>"
        };
        var Footer = {
            template: "<div>This is Footer</div>"
        };
        
        var routes = [
            {
                path: '/',
                components: {
                    default: Body,
                    header: Header,
                    footer: Footer
                }
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

<br>

### 2. 뷰 HTTP 통신

#### 웹 앱의 HTTP 통신 방법

* 뷰에서 ajax를 지원하기 위한 라이브러리르 제공.
* 뷰 프레임워크의 필수 라이브러리로 관리하던 뷰 리소스와 액시오스(axios)가 존재한다.

<br>

#### 뷰 리소스

* 코어 팀에서 초기에는 지원하였으나 현재는 공식 지원을 중단하기로 결정.
* 하지만, 아직 계속 사용할 수 있는 라이브러리.
```html
[버튼을 클릭하면 지정한 URL의 데이터를 가져오는 예제]
<body>
    <div id="app">
        <button v-on:click="getData">프레임워크 목록 가져오기</button>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue-resource@1.3.4"></script>
    <script>
        new Vue({
            el: '#app',
            methods: {
                getData: function() {
                    this.$http
                        .get('https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json')
                        .then(function(response){
                            console.log(response);
                            console.log(JSON.parse(response.data));
                        });
                }
            }
        });
    </script>
</body>
```

<br>

#### 액시오스 (Axios)

* 현재 뷰 커뮤니티에서 가장 많이 사용되는 HTTP 통신 라이브러리.
* Promise 기반의 API 형식이 다양하게 제공되어 별도의 로직을 구현할 필요 없이 간편하게 원하는 로직을 구현할 수 있다.

| API 유형                              | 처리 결과                                                                                       |
|---------------------------------------|------------------------------------------------------------------------------------------------|
| axios.get('URL 주소').then().catch()  | HTTP GET 요청.<br>성공하면 then(), 실패하면 catch() 실행.                                        |
| axios.post('URL 주소').then().catch() | HTTP POST 요청.<br>성공하면 then(), 실패하면 catch() 실행.                                       |
| axios({옵션속성})                     | HTTP 요청에 대한 속성들을 직접 정의하여 보낼 수 있다.<br>URL, HTTP 메소드, 보내는 데이터 유형 등등. |

```html
[버튼을 클릭하면 지정한 URL의 데이터를 가져오는 예제]
<body>
    <div id="app">
        <button v-on:click="getData">프레임워크 목록 가져오기</button>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script>
        new Vue({
            el: '#app',
            methods: {
                getData: function() {
                    axios
                        .get('https://raw.githubusercontent.com/joshua1988/doit-vuejs/master/data/demo.json')
                        .then(function(response){
                            console.log(response);
                        });
                }
            }
        });
    </script>
</body>
```