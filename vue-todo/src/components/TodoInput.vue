<template>
  <div class="inputBox shadow">
    <!-- v-model="특정 데이터 속성": input에 입력된 텍스트값을 동적으로 vue instance에 맵핑 -->
    <input
      type="text"
      v-model="newTodoItem"
      v-on:keyup.enter="addTodo"
      placeholder="Type what you have to do"
    >
    <span class="addContainer" v-on:click="addTodo">
      <i class="fas fa-plus addBtn"></i>
    </span>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      newTodoItem: ""
    };
  },
  methods: {
    addTodo: function() {
      // input값이 있어야 실행
      if (this.newTodoItem !== "") {
        // key / value 구분해서 로컬 스토리지에 저장
        var obj = {
          completed: false,
          item: this.newTodoItem,
        };
        // stringify(): 객체가 string으로 변환되어 저장(로컬 스토리지의 특성상 object-JSON 사용)
        // stringify()하지 않으면 js object 형태인 [object Object]로 들어가게 되어 값을 확인할 수 없다
        localStorage.setItem(this.newTodoItem, JSON.stringify(obj));
        this.clearInput();
      }
    },
    // 저장 후 input box 초기화
    clearInput: function() {
      this.newTodoItem = "";
    }
  }
};
</script>

<style scoped>
input:focus {
  outline: none;
}
.inputBox {
  background: #fff;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
}
.inputBox input {
  border-style: none;
  font-size: 0.9rem;
}
.addContainer {
  float: right;
  background: linear-gradient(to right, #6478fb, #8763fb);
  display: block;
  width: 3rem;
  border-radius: 0 5px 5px 0;
}
.addBtn {
  color: #fff;
  vertical-align: middle;
}
</style>
