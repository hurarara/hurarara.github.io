---
published: true
layout: post
title: "Vue.js todo-01 프로젝트 생성 및 컴포넌트 생성, 등록"
category: Vue.js
tags: Vue.js
comments: true
---

### vue_todo
먼저 [Vue 5장]({{site.url}}/vue.js/2019/01/02/vue-ch05-post.html)을 참조하여 `Vue 프로젝트`를 생성한다.   <br/>
`npm run dev` 명령어를 실행시키고 로컬 화면이 정상적으로 나온다면 뷰 CLI 프로젝트가 초기생성 성공   <br/>
![01]({{site.url}}/img/post/vue/todo/01.png){: width="90%" height="100%"}     <br/>
<br/>

#### 반응형 웹 디자인 태그 설정
반응형 웹 디자인은 하나의 웹 사이트로 PC, 태블릿, 모바일 등의 기기에서 자연스러운 레이아웃을 제공하는 웹 디자인 방법.   <br/>
index.html 파일에 다음 태그를 추가한다.   <br/>
~~~html
<head>
    <!-- width=device-width : 기기의 너비만큼 웹페이지의 너비를 지정 -->
    <!-- initial-scale=1.0 : 페이지의 비율 지정 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
~~~
<br/>

#### 어썸 아이콘 CSS 설정
\<head>태그 안에 \<link>태그를 추가한다.   <br/>
~~~html
<!-- 어썸아이콘 CSS -->
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.10/css/all.css">
~~~
<br/>

#### 폰트, 파비콘(로고) 설정
애플리케이션에서 사용할 폰트와 파비콘을 설정한다.   <br/>
구글에서 favicon generator를 검색하고 사이트에 들어가 이미지 파일을 파비콘으로 변환한다.    <br/>
~~~html
<!-- 파비콘 -->
<link rel="shortcut icon" href="/favicon.ico" type="image/x-icon">
<link rel="icon" href="/favicon.ico" type="image/x-icon">
<!-- Ubuntu 폰트 -->
<link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Ubuntu" />
~~~
<br/>

#### 컴포넌트 생성
`src 폴더` 밑에 `components 폴더` 생성, 그 밑에는 `TodoHeader.vue`, `TodoInput.vue`, `TodoList.vue`, `TodoFooter.vue` 생성  <br/>
컴포넌트를 이렇게 한 곳에 모아두면 관리하기 용의하다! <br/>
`규모가 커질 때는 components/기능/컴포넌트.vue와 같이 관리하자!` <br/>
아래는 `.vue`파일의 기본적인 구성이다. 생성한 .vue파일에 모두 아래의 코드를 삽입하고 \<template>태그에 각각 해당 \<div>태그를 통해 영역을 구분하도록 한다.
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->
    <div>header</div>
</template>

<script>
    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.
    }
</script>

<style>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */
</style>
~~~
<br/>

#### 컴포넌트 등록
`App.vue`를 최상위의 컴포넌트로 하고 아래와 같이 입력한다.   <br/>
아래의 .vue 파일의 주석을 삭제하지 않는 이유는 이 구성이 그만큼 중요하기 때문이다. 꼭 기억하고 있자.   <br/>
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <div id="app">
        <TodoHeader></TodoHeader>
        <TodoInput></TodoInput>
        <TodoList></TodoList>
        <TodoFooter></TodoFooter>
    </div>
</template>

<script>
    // 싱글 파일 컴포넌트 체계에서 다른 위치에 있는 컴포넌트를 불러올 때 다음과 같이 import 한다.
    // ES6 방식 : import 구문으로 컴포넌트의 내용을 불러와 담고 넘겨준다.
    import TodoHeader from './components/TodoHeader.vue'
    import TodoInput from './components/TodoInput.vue'
    import TodoList from './components/TodoList.vue'
    import TodoFooter from './components/TodoFooter.vue'

    // ES5 방식 : var로 선언한 객체에 컴포넌트 내용을 담고 넘겨준다.
    // var TodoHeader = {
    //     template : '<div>header</div>'
    // };

    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.
        
        // 지역 컴포넌트 등록
        components : {
            'TodoHeader' : TodoHeader,
            'TodoInput' : TodoInput,
            'TodoList' : TodoList,
            'TodoFooter' : TodoFooter
        }
    }
</script>

<style>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */
</style>
~~~
<br/>
