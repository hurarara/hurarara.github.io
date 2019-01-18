---
published: true
layout: post
title: "Vue.js todo-03 컴포넌트 기능 개선"
category: Vue.js
tags: Vue.js
comments: true
---

### vue_todo
생성한 각 컴포넌트의 역할은 다음과 같다.    <br/>
**:: 상위 컴포넌트 ::**  <br/>
`App` : 로컬스토리지에서의 데이터를 각 하위 컴포넌트로 전달  <br/>
**:: 하위 컴포넌트 ::**  <br/>
`TodoHeader` : 애플리케이션 이름 표시   <br/>
`TodoInput` : 할 일 입력 및 추가(이벤트를 상위 컴포넌트로 전달)   <br/>
`TodoList` : 할 일 목록 표시 및 특정 할 일 삭제(이벤트를 상위 컴포넌트로 전달)   <br/>
`TodoFooter` : 할 일 모두 삭제 (이벤트를 상위 컴포넌트로 전달)  <br/>
<br/>

#### 컴포넌트 기능 개선하기.
**[Vue.js todo-02 컴포넌트 내용 구현]({{site.url}}/vue.js/2019/01/15/vue-todo02-post.html)**까지 진행한다면 문제점이 2가지가 나온다.    <br/>

1. 할 일을 입력하고 추가했을 때 화면에 바로 적용되지 않는다.    <br/>
2. 저장했던 데이터를 한 번에 모두 삭제 했을 때 화면에 바로 적용되지 않는다.    <br/>

만약 컴포넌트를 현재와 같이 4개로 분리하지 않고 `한 컴포넌트에서 작업이 되도록` 했다면 화면에 바로 적용되었을 것이다. <br/>
하지만 이를 위해서 기능이 있는 곳마다 컴포넌트를 따로 두고 그 컴포넌트들만 관리하면 된다는 이점을 버리기엔 너무나 아깝다.  <br/>
그렇기에 이 문제를 해결하기 위해 `로컬스토리지`에서 최상위 컴포넌트인 `App.vue`로, `App.vue`에서 각 하위 컴포넌트로 데이터를 전달해줄 수 있도록 한다.  <br/>
**App.vue**
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <div id="app">
        <TodoHeader></TodoHeader>
        <!-- 이벤트 전달 받음-->
        <!-- inputAddTodo : TodoInput.vue에서 넘어오는 이벤트명, appAddTodo : App.vue의 메소드 -->
        <TodoInput v-on:inputAddTodo="appAddTodo"></TodoInput>
        <!-- 데이터 전달 함-->
        <!-- propsdata : TodoList.vue로 전달할 파라미터명, todoItems : App.vue에서 선언된 변수-->
        <TodoList v-bind:propsdata="todoItems" v-on:listRemoveTodo="appRemoveTodo"></TodoList>
        <TodoFooter v-on:footRemoveAll="appRemoveAll"></TodoFooter>
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
        
        // todoItems 선언
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
            // TodoInput.vue에서 이벤트를 통해 전달받은 value를 todoItem으로 가져옴
            appAddTodo(todoItem) {
                // 로컬스토리지에 데이터를 추가하는 로직
                localStorage.setItem(todoItem, todoItem);
                this.todoItems.push(todoItem);
            },
            appRemoveTodo(todoItem, index) {
                // 로컬스토리지에 아이템을 삭제
                localStorage.removeItem(todoItem);
                // splice() : 자바스크립트에 기본적으로 내장되있는 API. 배열의 특정 인덱스에서 부여한 숫자만큼의 인덱스를 삭제.
                // todoItems 배열에서 인덱스를 삭제
                this.todoItems.splice(index, 1);
            },
            appRemoveAll() {
                localStorage.clear();
                this.todoItems = [];
            }
        },
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

**TodoInput.vue**
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <!-- 할 일을 입력하기 위한 인풋박스와 버튼을 추가 -->
    <div class="inputBox shadow">
        <!-- v-model, v-on 속성을 통해 뷰에서 인식할 수 있도록 한다. -->
        <!-- placeholder : 인풋 박스의 힌트 속성, v-on:keyup.enter : 인풋 박스에서 엔터키를 눌렀을 때 발생하는 이벤트 속성 -->
        <input type="text" v-model="newTodoItem" placeholder="Type what you have to do" v-on:keyup.enter="inputAddTodo">
        <!-- <span> : <button> 태그 대신 클릭 이벤트를 받을 태그 -->
        <span class="addContainer" v-on:click="inputAddTodo">
            <!-- <i class="addBtn fas fa-plus"> : 어썸 아이콘의 + 아이콘을 추가 -->
            <i class="addBtn fas fa-plus" aria-hidden="true"></i>
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
            inputAddTodo() {
                if(this.newTodoItem !== "") {
                    var value = this.newTodoItem.trim();

                    // setItem()의 형식은 키, 값이다.
                    // localStorage.setItem(value, value);  //기능 개선을 위해 삭제
                    this.$emit('inputAddTodo', value);   //기능 개선을 위해 추가

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
    /* inputBox 클래스 안의 <input>태그에 테두리, 폰트 속성 적용 */
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
        cursor: pointer;    /* 마우스 포인터 변경 */
    }
    /* 아이콘의 + 색, 수직정렬 적용 */
    .addBtn {
        color : white;
        vertical-align : middle;
    }
</style>
~~~
<br/>

**TodoList.vue**
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <section>
        <ul>
            <!-- v-for 디렉티브를 이용해 아이템의 개수만큼 반복 -->
            <!-- v-for : 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복 출력. -->
            <!-- index : v-for 디렉티브에서 기본적으로 제공하는 변수 -->
            <!-- ESLint 플러그인으로 인해 에러가 기존은 맞지않는 문법이라고 나옴. 그래서 :key="todoItem" 코드를 추가 -->
            <li v-for="(todoItem, index) in propsdata" :key="todoItem" class="shadow">
                <!-- 체크 아이콘 -->
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                {{ todoItem }}
                
                <!-- 삭제이벤트를 적용해주기 위한 영역 -->
                <!-- @click은 v-on:click과 동일하게 작용 -->
                <span class="removeBtn" type="button" @click="listRemoveTodo(todoItem, index)">
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

        // App.vue에서 propsdata를 통해 데이터를 전달받는다.
        props : ['propsdata'],

        // 로컬 스토리지의 데이터를 뷰 데이터로 저장한다.
        // 기능 개선에서 propsdata를 받기때문에 불필요해짐
        // data() {
        //     return {
        //         todoItems : []
        //     }
        // },
        // // 라이프사이클 created()
        // created() {
        //     if(localStorage.length > 0) {
        //         // 로컬 스토리지에 저장된 데이터를 한 번에 불러올 수 있는 API는 없어서 반복문을 돌린다.
        //         for(var i = 0; i < localStorage.length; i++) {
        //             this.todoItems.push(localStorage.key(i));
        //         }
        //     }
        // },

        methods : {
            listRemoveTodo(todoItem, index) {
                // 로컬스토리지에 아이템을 삭제
                // localStorage.removeItem(todoItem);   //기능 개선을 위해 삭제
                // splice() : 자바스크립트에 기본적으로 내장되있는 API. 배열의 특정 인덱스에서 부여한 숫자만큼의 인덱스를 삭제.
                // todoItems 배열에서 인덱스를 삭제
                // this.todoItems.splice(index, 1); //기능 개선을 위해 삭제
                this.$emit('listRemoveTodo', todoItem, index);   //기능 개선을 위해 추가
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
        cursor: pointer;    /* 마우스 포인터 변경 */
    }
</style>
~~~
<br/>

**TodoFooter.vue**
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->


    <div class="clearAllContainer">
        <span class="clearAllBtn" @click="footRemoveAll">Clear All</span>
    </div>
</template>

<script>
    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.

        methods : {
            footRemoveAll() {
                // localStorage.clear();   //기능 개선을 위해 삭제
                this.$emit('footRemoveAll');   //기능 개선을 위해 추가
            }
        }
    }
</script>

<style scoped>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    /* Clear All 버튼의 영역 */
    .clearAllContainer {
        width: 8.5rem;
        height: 50px;
        line-height: 50px;
        background-color: white;
        border-radius: 5px;
        margin: 0 auto;
        cursor: pointer;    /* 마우스 포인터 변경 */
    }
    /* Clear All 버튼 속성 */
    .clearAllBtn {
        color: #e20303;
        display: black;
    }
</style>
~~~
<br/>
`v-on`, `$emit()`을 이용해 하위 컴포넌트(TodoInput.vue, TodoList.vue, TodoFooter.vue)로부터 상위 컴포넌트(App.vue)가 이벤트를 통해 데이터를 전달받고, `v-bind`에 `props`속성을 이용해 상위 컴포넌트(App.vue)로부터 하위 컴포넌트(TodoInput.vue, TodoList.vue, TodoFooter.vue)로 데이터를 전달한다. <br/>
즉 TodoList.vue를 제외한 `하위 컴포넌트들은 이벤트만을 발생시키고 내부적인 데이터의 수정은 App.vue에서 이루어지도록 한다.`    <br/>

`props` : 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달할 때 사용하는 속성. <br/>
`$emit()` : 이벤트 발생시켜 상위 컴포넌트로 이벤트와 데이터를 전달. 상위 컴포넌트는 `$emit()`을 통해 받은 데이터에 대한 직접적인 변경이 없도록 해야한다.   <br/>