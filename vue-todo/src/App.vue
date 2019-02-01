<template>
  <div id="app">
      <todo-header></todo-header>
      <todo-input v-on:addTodoItem="addOneItem"></todo-input>
      <todo-list v-bind:propsdata="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem"></todo-list>
      <todo-footer v-on:clearAll="clearAllItems"></todo-footer>
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
  //////// Refactoring: App.vue 한 개의 컴포넌트에서 모든 데이터 조작이 이루어지도록 변경 ////////
  methods:{
    addOneItem: function(todoItem){
      // key / value 구분해서 로컬 스토리지에 저장
      const obj = {
        completed: false,
        item: todoItem,
      };
      // stringify(): 객체가 string으로 변환되어 저장(로컬 스토리지의 특성상 object-JSON 사용)
      // stringify()하지 않으면 js object 형태인 [object Object]로 들어가게 되어 값을 확인할 수 없다
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem: function(todoItem, index){
      localStorage.removeItem(todoItem);
      // splice(): 특정 인덱스에서 하나를 지우는 js API. 기존 배열을 변경하여 새로운 배열 반환
      // cf) slice(): 삭제하되 기존 배열은 변경하지 않음
      this.todoItems.splice(index, 1);
      console.log('removed');
    },
    toggleOneItem: function(todoItem, index){
      // todoItem 개별적으로 접근하지 않고 todoItems의 요소로 접근(컴포넌트 간의 경계를 좀 더 명확하게 함)
      this.todoItems[index].completed = !this.todoItems[index].completed;

      // 로컬 스토리지의 데이터를 갱신
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems: function(){
      localStorage.clear();
      this.todoItems = [];
    }

  },
  created: function() { 
    if (localStorage.length > 0) {
      for (let i = 0; i < localStorage.length; i++) {

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
