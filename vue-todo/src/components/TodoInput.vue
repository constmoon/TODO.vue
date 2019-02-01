<template>
  <div class="inputBox shadow">
    <!-- v-model="특정 데이터 속성": input에 입력된 텍스트값을 동적으로 vue instance에 맵핑 -->
    <input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo" placeholder="할 일을 입력하세요">
    <span class="addContainer" v-on:click="addTodo">
      <i class="fas fa-plus addBtn"></i>
    </span>

    <!-- 한 개의 템플릿 안에는 한 개의 root 태그만 있어야 한다 -->
    <Modal v-if="showModal" @close="showModal = false">
      <h3 slot="header">경고!
        <i class="cloaseModalBtn fas fa-times" @click="showModal = false"></i>
      </h3>
      <p slot="body">무엇인가를 입력하세요</p>
    </Modal>
  </div>
</template>

<script>
import Modal from "./common/Modal.vue";

export default {
  data() {
    return {
      newTodoItem: "",
      showModal: false
    };
  },
  methods: {
    addTodo() {
      // input값이 있어야 실행
      if (this.newTodoItem !== "") {
        // input: 이벤트 발생
        this.$emit("addTodoItem", this.newTodoItem);
        this.clearInput();
      }
      else{
        this.showModal = !this.showModal;
      }
    },
    // 저장 후 input box 초기화
    clearInput() {
      this.newTodoItem = "";
    }
  },
  // input의 상위 컴포넌트가 App.vue, 하위 컴포넌트가 Modal
  components: {
    // Modal: Modal 과 같음
    Modal
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
.closeModalBtn{
  color: #42b983;
}
</style>
