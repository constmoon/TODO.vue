<template>
  <div>
    <ul>
      <!-- v-bind:key => todoItem의 텍스트가 key가 되어 중복 X, v-for의 성능 가속화 -->
      <li v-for="(todoItem,index) in propsdata" v-bind:key="todoItem.item" class="shadow">
        <i class="checkBtn fas fa-check" v-bind:class="{checkBtnCompleted: todoItem.completed}" 
        v-on:click="toggleCompltete(todoItem, index)"></i>
        <span v-bind:class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
        <span class="removeBtn" v-on:click="removeTodo(todoItem,index)">
          <i class="fas fa-trash-alt"></i>
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['propsdata'],
  methods: {
    removeTodo: function(todoItem, index) {
      localStorage.removeItem(todoItem);
      // splice(): 특정 인덱스에서 하나를 지우는 js API. 기존 배열을 변경하여 새로운 배열 반환
      // cf) slice(): 삭제하되 기존 배열은 변경하지 않음
      this.todoItems.splice(index, 1);
    },
    toggleCompltete: function(todoItem, index){
        todoItem.completed = !todoItem.completed;
        // 로컬 스토리지의 데이터를 갱신
        localStorage.removeItem(todoItem.item);
        localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    }
  },
};
</script>

<style>
ul {
  list-style-type: none;
  padding-left: 0;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  line-height: 50px;
  height: 50px;
  min-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: #fff;
  border-radius: 5px;
}
.removeBtn {
  margin-left: auto;
  color: #de4343;
}
.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 20px;
}
.checkBtnCompleted {
  color: #b3adad;
}
.textCompleted {
  text-decoration: line-through;
  color: #b3adad;
}
</style>
