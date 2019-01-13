# 개발 환경 설정 및 첫 번째 프로젝트

### 1. 뷰 학습을 위한 개발 환경 설정하기

---

* 크롬 브라우저 설치
* 에디터 설치
* Node.js 설치
* 뷰 개발자 도구 설치

<br>

### 2. Hello Vue.js! 프로젝트 만들기

#### 뷰 시작하기

---

* HTML 파일 생성 → 뷰 소스 코드 추가 → 브라우저에서 실행

```HTML
<div id="app">{{ message }}</div>
<script src="https://cnd/jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        }
    })
</script>

```

<br>

#### 크롬 개발자 도구로 코드 확인하기

---

* 뷰 라이브러리를 정상적으로 로딩하였는지 확인 

<br>

#### 뷰 개발자 도구로 코드 확인하기

---

* 컴포넌트로 구성된 애플리에키션의 구조를 한눈에 확인