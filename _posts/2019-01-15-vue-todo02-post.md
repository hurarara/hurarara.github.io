---
published: true
layout: post
title: "Vue.js todo-02 컴포넌트 내용 구현"
category: Vue.js
tags: Vue.js
comments: true
---

### vue_todo
생성한 각 컴포넌트의 역할은 다음과 같다.    <br/>
`TodoHeader` : 애플리케이션 이름 표시   <br/>
`TodoInput` : 할 일 입력 및 추가   <br/>
`TodoList` : 할 일 목록 표시 및 특정 할 일 삭제   <br/>
`TodoFooter` : 할 일 모두 삭제   <br/>
먼저 TodoHeader.vue 파일부터 구현한다.  <br/>
<br/>

#### 애플리케이션 제목 구현하기. TodoHeader 컴포넌트
`App.vue` 파일에는 아래와 같이 CSS를 적용한다.  <br/>
~~~html
<style>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    /* App.vue CSS */
    /* 애플리케이션 전체의 텍스트 정렬 방식과 배경색 지정 */
    body {
        text-align : center;
        background-color : #F6F6F8;
    }
    /* 인풋 박스의 테두리 모양 정의 */
    input {
        border-style : groove;
        width : 200px;
    }
    /* 버튼의 테두리 모양 정의 */
    button {
        border-style : groove;
    }
    /* 그림자 정의 */
    /* .과 #의 차이 : #은 id, .은 클래스 */
    .shadow {
        box-shadow : 5px 10px 10px rgba(0, 0, 0, 0,03)
    }
</style>
~~~
<br/>
`TodoHeader.vue` 파일에는 아래와 같이 template과 CSS를 적용한다.    <br/>
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <!-- 제목을 표시한다. -->
    <header>
        <h1>Todo It!</h1>
    </header>
</template>

<script>
    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.
    }
</script>

<!-- scoped : 뷰의 속성으로 스타일 정의를 해당 컴포넌트에만 적용한다는 의미. -->
<style scoped>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    /* 제목의 텍스트 색, 굵기, 여백 지정 */
    h1 {
        color : #2F3B52;
        font-weight : 900;
        margin : 2.5rem 0 1.5rem;
    }
</style>
~~~
<br/>

#### 애플리케이션 입력 구현하기. TodoInput 컴포넌트
`TodoInput.vue` 파일에는 아래와 같이 적용한다.    <br/>
`v-model` : Form에서 주로 사용하는 속성. 폼에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화시킨다. \<input>, \<select>, \<textarea> 태그에만 사용 가능.   <br/>
`v-on` : `v-on:click`과 같이 화면 요소의 이벤트를 감지하여 처리할 때 사용.  <br/>
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <!-- 할 일을 입력하기 위한 인풋박스와 버튼을 추가 -->
    <div class="inputBox shadow">
        <!-- v-model, v-on 속성을 통해 뷰에서 인식할 수 있도록 한다. -->
        <!-- placeholder : 인풋 박스의 힌트 속성, v-on:keyup.enter : 인풋 박스에서 엔터키를 눌렀을 때 발생하는 이벤트 속성 -->
        <input type="text" v-model="newTodoItem" placeholder="Type what you have to do" v-on:keyup.enter="addTodo">
        <!-- <span> : <button> 태그 대신 클릭 이벤트를 받을 태그 -->
        <span class="addContainer" v-on:click="addTodo">
            <!-- <i class="addBtn fas fa-plus"> : 어썸 아이콘의 + 아이콘을 추가 -->
            <i class="addBtn fas fa-plus" area-hidden="true"></i>
        </span>
    </div>
</template>

<script>
    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.

        // 인풋 박스의 텍스트 값을 인식
        data() {
            return {
                newTodoItem : ''
            }
        },
        // 이벤트에 대한 로직
        methods : {
            // 로컬 스토리지에 데이터를 추가한다.
            // 추가 버튼이 클릭되면 인풋 박스에 값이 있을 때만 저장하고 인풋 박스를 clear 하도록 한다.
            addTodo() {
                if(this.newTodoItem !== "") {
                    var value = this.newTodoItem.trim();
                    // setItem()의 형식은 키, 값이다.
                    localStorage.setItem(value, value);
                    // 단일 책임 원칙(Single Responsibility Principle) : 함수 하나가 하나의 기능을 담당하도록하는 디자인 패턴
                    // this.newTodoItem = ''; 정도는 직접 이 부분에서 해줄 수 있으나 단일 책임원칙 디자인 패턴의 원칙에 따라 작성
                    this.clearInput();
                }
            },
            clearInput() {
                this.newTodoItem = '';
            }

            // 기존로직
            // addTodo() {
            // !== : 변수타입까지 고려하여 비교
            //     if(this.newTodoItem !== "") {
            //         var value = this.newTodoItem && this.newTodoItem.trim();
            //         // setItem()의 형식은 키, 값이다.
            //         localStorage.setItem(value, value);
            //         this.clearInput();
            //     }
            // }
        }
    }
</script>

<style scoped>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    /* 인풋 박스에 포커스가 갈 때 선 스타일 지정 */
    input:focus {
        outline : none;
    }
    /* inputBox class에 배경색, 높이, 텍스트의 중앙정렬을 위한 라인높이, 둥근테두리 적용 */
    .inputBox {
        background : white;
        height : 50px;
        line-height : 50px;
        border-radius : 5px;
    }
    /* inputBox 클래스 안의 input태그에 테두리, 폰트 속성 적용 */
    .inputBox input {
        border-style : none;
        font-size : 0.9rem;
    }
    /* 아이콘의 배경 부분. 우측배치, 배경색(그라데이션), 너비, 둥근테두리 적용 */
    .addContainer {
        float : right;
        background : linear-gradient(to right, #6478FB, #8763FB);
        display : block;
        width : 3rem;
        border-radius : 0 5px 5px 0;
    }
    /* 아이콘의 + 색, 수직정렬 적용 */
    .addBtn {
        color : white;
        vertical-align : middle;
    }
</style>
~~~
<br/>

#### 할 일 목록 만들기. TodoList 컴포넌트
`로컬 스토리지`에 저장했던 데이터를 뷰 데이터에 저장하고, 리스트로 표현하고, 삭제한다.    <br/>
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <section>
        <ul>
            <!-- v-for 디렉티브를 이용해 아이템의 개수만큼 반복 -->
            <!-- v-for : 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복 출력. -->
            <!-- index : v-for 디렉티브에서 기본적으로 제공하는 변수 -->
            <li v-for="(todoItem, index) in todoItems" class="shadow">
                <!-- 체크 아이콘 -->
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                {{ todoItem }}
                
                <!-- 삭제이벤트를 적용해주기 위한 영역 -->
                <!-- @click은 v-on:click과 동일하게 작용 -->
                <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)">
                    <!-- 휴지통 아이콘 -->
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
            </li>
        </ul>
    </section>
</template>

<script>
    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.

        // 로컬 스토리지의 데이터를 뷰 데이터로 저장한다.
        data() {
            return {
                todoItems : []
            }
        },
        // 라이프사이클 created()
        created() {
            if(localStorage.length > 0) {
                // 로컬 스토리지에 저장된 데이터를 한 번에 불러올 수 있는 API는 없어서 반복문을 돌린다.
                for(var i = 0; i < localStorage.length; i++) {
                    this.todoItems.push(localStorage.key(i));
                }
            }
        },
        methods : {
            removeTodo(todoItem, index) {
                // 로컬스토리지에 아이템을 삭제
                localStorage.removeItem(todoItem);
                // splice() : 자바스크립트에 기본적으로 내장되있는 API. 배열의 특정 인덱스에서 부여한 숫자만큼의 인덱스를 삭제.
                // todoItems 배열에서 인덱스를 삭제
                this.todoItems.splice(index, 1);
            }
        }
    }
</script>

<style scoped>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    /* 목록 스타일 */
    ul {
        list-style-type: none;  /* 목록 아이템의 스타일 */
        padding-left: 0px;
        margin-top: 0;
        text-align: left;
    }
    /* 항목 스타일 */
    li {
        display: flex;  /* flex : 비율 기준의 레이아웃 방식 */
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        background: white;
        border-radius: 5px;
    }
    /* 체크아이콘 */
    .checkBtn {
        line-height: 45px;
        color: #62acde;
        margin-right: 5px;
    }
    /* 휴지통아이콘 */
    .removeBtn {
        margin-left: auto;
        color: #de4343;
    }
</style>
~~~
`*v-for` : 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복 출력. <br/>
`*created` : data, methods 속성이 정의된 후로 해당 속성의 대한 로직을 실행 가능하다. 서버에 데이터를 요청해 받아오는 로직을 수행하기 좋은 단계이다. DOM 요소에 접근할 수 없는 단계  <br/>
<br/>

#### 모두 삭제버튼 만들기. TodoFooter 컴포넌트
`로컬 스토리지`에 저장했던 데이터와 `뷰 데이터`를 한 번에 삭제한다.    <br/>
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <section>
        <ul>
            <!-- v-for 디렉티브를 이용해 아이템의 개수만큼 반복 -->
            <!-- v-for : 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복 출력. -->
            <!-- index : v-for 디렉티브에서 기본적으로 제공하는 변수 -->
            <li v-for="(todoItem, index) in todoItems" class="shadow">
                <!-- 체크 아이콘 -->
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                {{ todoItem }}
                
                <!-- 삭제이벤트를 적용해주기 위한 영역 -->
                <!-- @click은 v-on:click과 동일하게 작용 -->
                <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)">
                    <!-- 휴지통 아이콘 -->
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
            </li>
        </ul>
    </section>
</template>

<script>
    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.

        // 로컬 스토리지의 데이터를 뷰 데이터로 저장한다.
        data() {
            return {
                todoItems : []
            }
        },
        // 라이프사이클 created()
        created() {
            if(localStorage.length > 0) {
                // 로컬 스토리지에 저장된 데이터를 한 번에 불러올 수 있는 API는 없어서 반복문을 돌린다.
                for(var i = 0; i < localStorage.length; i++) {
                    this.todoItems.push(localStorage.key(i));
                }
            }
        },
        methods : {
            removeTodo(todoItem, index) {
                // 로컬스토리지에 아이템을 삭제
                localStorage.removeItem(todoItem);
                // splice() : 자바스크립트에 기본적으로 내장되있는 API. 배열의 특정 인덱스에서 부여한 숫자만큼의 인덱스를 삭제.
                // todoItems 배열에서 인덱스를 삭제
                this.todoItems.splice(index, 1);
            }
        }
    }
</script>

<style scoped>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    /* 목록 스타일 */
    ul {
        list-style-type: none;  /* 목록 아이템의 스타일 */
        padding-left: 0px;
        margin-top: 0;
        text-align: left;
    }
    /* 항목 스타일 */
    li {
        display: flex;  /* flex : 비율 기준의 레이아웃 방식 */
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        background: white;
        border-radius: 5px;
    }
    /* 체크아이콘 */
    .checkBtn {
        line-height: 45px;
        color: #62acde;
        margin-right: 5px;
    }
    /* 휴지통아이콘 */
    .removeBtn {
        margin-left: auto;
        color: #de4343;
    }
</style>
~~~
<br/>

여기까지 진행한다면 문제점이 2가지가 나온다.    <br/>
1. 할 일을 입력하고 추가했을 때 화면에 바로 적용되지 않는다.    <br/>
2. 저장했던 데이터를 한 번에 모두 삭제 했을 때 화면에 바로 적용되지 않는다.    <br/>

만약 컴포넌트를 현재와 같이 4개로 분리하지 않고 `한 컴포넌트에서 작업이 되도록` 했다면 화면에 바로 적용되었을 것이다. <br/>
하지만 이를 위해서 기능이 있는 곳마다 컴포넌트를 따로 두고 그 컴포넌트들만 관리하면 된다는 이점을 버리기엔 너무나 아깝다.  <br/>
그렇기에 이 문제를 해결하기 위해 `로컬스토리지`에서 최상위 컴포넌트인 `App.vue`로, `App.vue`에서 각 하위 컴포넌트로 데이터를 전달해줄 수 있도록 한다.  <br/>