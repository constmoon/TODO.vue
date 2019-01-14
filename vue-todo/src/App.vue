<template>
  <div id="app">
      <todo-header></todo-header>
      <todo-input v-on:addTodo="addTodo"></todo-input>
      <todo-list v-bind:propsdata="todoItems"></todo-list>
      <todo-footer></todo-footer>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';
import TodoFooter from './components/TodoFooter.vue';


export default {
  // 모든 컴포넌트에서 사용되는 데이터
  data: function(){
    return{
      todoItems: []
    }
  },
  methods:{
    addTodo: function(todoItem){
      // key / value 구분해서 로컬 스토리지에 저장
        var obj = {
          completed: false,
          item: todoItem,
        };
        // stringify(): 객체가 string으로 변환되어 저장(로컬 스토리지의 특성상 object-JSON 사용)
        // stringify()하지 않으면 js object 형태인 [object Object]로 들어가게 되어 값을 확인할 수 없다
        localStorage.setItem(todoItem, JSON.stringify(obj));
        this.todoItems.push(obj);
    }
  },
  created: function() {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {

        // webpack dev server를 제외한 나머지들 저장
        if (localStorage.key(i) !== "loglevel:webpack-dev-server") {
           this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));          
        }
      }
    }
  },
  components:{
    // 컴포넌트 태그명 : 컴포넌트 내용
    'todo-header' : TodoHeader,
    'todo-input' : TodoInput,
    'todo-list' : TodoList,
    'todo-footer' : TodoFooter,
  }
}
</script>

<style>
body{
  text-align: center;
  background-color: #f6f6f6;
}
input{
  border-style: groove;
  width: 200px;
}
button{
  border-style: groove;
}
.shadow{
  box-shadow: 5px 10px 10px rgba(0,0,0,0.03);
}
</style>
