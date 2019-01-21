---
published: true
layout: post
title: "Vue.js todo-04 완성도 높이기"
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

#### 완성도 높이기.
**[Vue.js todo-03 컴포넌트 기능 개선]({{site.url}}/vue.js/2019/01/18/vue-todo03-post.html)**까지 진행하여 어지간한 메인 기능은 다 구현되었다.    <br/>
하지만 갑자기 데이터가 추가되고, 사라지기때문에 뭔가 많이 밋밋해 보인다. 애니메이션과 모달을 이용해 더욱 완성도를 높일 수 있다.    <br/>
<br/>

#### 뷰 애니메이션
`뷰 애니메이션`은 뷰 프레임워크 자체에서 지원하는 애니메이션 기능이다.  <br/>
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <section>
        <!-- transition-group : 목록에 애니메이션을 추가할 때 사용되는 태그, tag : 애니메이션이 들어갈 html 태그 이름 -->
        <!-- 애니메이션 추가 -->
        <transition-group name="list" tag="ul">
            <!-- v-for 디렉티브를 이용해 아이템의 개수만큼 반복 -->
            <!-- v-for : 지정한 뷰 데이터의 개수만큼 해당 HTML 태그를 반복 출력. -->
            <!-- index : v-for 디렉티브에서 기본적으로 제공하는 변수 -->
            <!-- ESLint 플러그인으로 인해 에러가 기존은 맞지않는 문법이라고 나옴. 그래서 :key="todoItem" 코드를 추가 -->
            <!-- :key : v-for 디렉티브를 사용할 때 지정하는게 좋으며, 아이템의 순서조정에 대한 돔 이동을 최소화하여 화면을 더 빨리 그릴 수 있도록 한다. -->
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
        </transition-group>
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
    /* 효과 추가 부분 */
    /* 데이터가 들어오고 나가는 타임 */
    .list-enter-active, .list-leave-active {
        transition: all 1s;
    }
    /* 데이터가 들어오고 나갈 때 효과 */
    .list-enter, .list-leave-to {
        opacity: 0;
        transform: translate(30px);
    }
</style>
~~~
`transition-group` : 목록에 애니메이션을 추가할 때 사용되는 태그, tag : 애니메이션이 들어갈 html 태그 이름  <br/>
`:key` : `v-for` 디렉티브를 사용할 때 지정하는게 좋으며, 아이템의 순서조정에 대한 돔 이동을 최소화하여 화면을 더 빨리 그릴 수 있도록 한다.  <br/>
<br/>

#### 뷰 모달
현재까지 만든 화면은 기초적인 `Validation Check`가 되지 않고 있다. 아무것도 입력하지 않고 추가버튼을 눌러보면 알 수 있다.  <br/>
그래서 뷰에서 제공하는 팝업 대화상자인 `모달(modal)`을 이용해 이에 대한 예외처리를 하기로 했다.   <br/>
**Modal.vue**
~~~html
<template>
    <!-- HTML 태그 내용 -->
    <!-- 화면에 표시할 요소들을 정의하는 영역, HTML+뷰 데이터 바인딩 -->

    <!-- template for the modal component -->
    <transition name="modal">
        <div class="modal-mask">
        <div class="modal-wrapper">
            <div class="modal-container">

            <div class="modal-header">
                <slot name="header">
                </slot>
            </div>

            <div class="modal-body">
                <slot name="body">
                </slot>
            </div>

            <div class="modal-footer">
                <slot name="footer">
                <button class="modal-default-button" @click="$emit('close')">
                    OK
                </button>
                </slot>
            </div>
            </div>
        </div>
        </div>
    </transition>
</template>

<script>
    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.

    }
</script>

<style scoped>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    .modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        display: table;
        transition: opacity .3s ease;
    }
    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
    }
    .modal-container {
        width: 300px;
        margin: 0px auto;
        padding: 20px 30px;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }
    .modal-header {
        margin-top: 0;
        color: #42b983;
    }
    .modal-body {
        margin: 20px 0;
    }
    .modal-default-button {
        float: center;
        cursor: pointer;
    }

    /*
    * The following styles are auto-applied to elements with
    * transition="modal" when their visibility is toggled
    * by Vue.js.
    *
    * You can easily play with the modal transition by editing
    * these styles.
    */
    .modal-enter {
        opacity: 0;
    }
    .modal-leave-active {
        opacity: 0;
    }
    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }
</style>
~~~
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

        <!-- v-if : false 값을 가지면 해당 HTML 태그를 화면에 표시하거나 표시하지 않음.  -->
        <!-- @close : close 이벤트 -->
        <modal v-if="showModal" @close="showModal = false" v-on:close="close">
            <!-- 모달 Header -->
            <h3 slot="header">경고</h3>
            <!-- 모달 Body -->
            <span slot="body">
                할 일을 입력하세요.
                <!-- <i class="closeModalBtn fas fa-times" aria-hidden="true"></i> -->
            </span>
        </modal>
    </div>
</template>

<script>
    // Modal.vue를 컴포넌트로 등록
    import Modal from './common/Modal.vue'

    export default {
        // 자바 스크립트 내용
        // 뷰 컴포넌트의 내용을 정의하는 영역, template, data, methods 등
        // export default {} 는 ES6의 자바스크립트 모듈화와 관련된 문법.

        // 인풋 박스의 텍스트 값을 인식
        data() {
            return {
                newTodoItem : '',
                showModal : false   //모달 동작을 위한 플래그 값
            }
        },
        // 이벤트에 대한 로직
        methods : {
            // 로컬 스토리지에 데이터를 추가한다.
            // 추가 버튼이 클릭되면 인풋 박스에 값이 있을 때만 저장하고 인풋 박스를 clear 하도록 한다.
            inputAddTodo() {
                if(this.newTodoItem !==  "") {
                    var value = this.newTodoItem.trim();

                    // setItem()의 형식은 키, 값이다.
                    // localStorage.setItem(value, value);  //기능 개선을 위해 삭제
                    this.$emit('inputAddTodo', value);   //기능 개선을 위해 추가

                    // 단일 책임 원칙(Single Responsibility Principle) : 함수 하나가 하나의 기능을 담당하도록하는 디자인 패턴
                    // this.newTodoItem = ''; 정도는 직접 이 부분에서 해줄 수 있으나 단일 책임원칙 디자인 패턴의 원칙에 따라 작성
                    this.clearInput();
                }
                else {
                    // 텍스트 미입력 시 모달 동작
                    this.showModal = !this.showModal;
                }

            },
            clearInput() {
                this.newTodoItem = '';
            },
            close() {
                'Modal'.close;
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
        },
        components : {
            'Modal' : Modal   //모달 등록
        }
    }
</script>

<style scoped>
    /* CSS 스타일 내용 */
    /* 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역 */

    /* 인풋 박스에 포커스가 갈 때 선 스타일 지정 */
    input:focus {
        outline : tomato;
        outline-style: auto;
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
        border-style: none;
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