<template>
  <div>
    <ul>
      <!-- v-bind:key => todoItem의 텍스트가 key가 되어 중복 X, v-for의 성능 가속화 -->
      <li v-for="(todoItem,index) in todoItems" v-bind:key="todoItem.item" class="shadow">
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
  // 로컬 스토리지에서 꺼내서 담을 데이터
  data: function() {
    return {
      todoItems: []
    };
  },
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
  // 인스턴스가 생성되자마자 호출됨
  created: function() {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {

        // webpack dev server를 제외한 나머지들 저장
        if (localStorage.key(i) !== "loglevel:webpack-dev-server") {
            // JSON.parse()로 value 리턴, todoItems에 추가
           this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
            
        }
        
      }
    }
  }
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
