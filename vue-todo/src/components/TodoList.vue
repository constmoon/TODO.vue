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
      // 2개의 파라미터를 App.vue의 removeItem으로 보냄
      // $emit(eventName)을 사용하여 이벤트를 발생 
      this.$emit('removeItem', todoItem, index);
    },
    toggleCompltete: function(todoItem, index){
      this.$emit('toggleItem', todoItem, index);
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
