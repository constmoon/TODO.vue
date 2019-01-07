<template>
    <div>
        <!-- v-model="특정 데이터 속성": input에 입력된 텍스트값을 동적으로 vue instance에 맵핑 -->
        <input type="text" v-model="newTodoItem">
        <button v-on:click="addTodo">add</button>
    </div>
</template>

<script>
export default {
    data: function(){
        return{
            newTodoItem: ""
        }
    },
    methods:{
        addTodo: function(){
            localStorage.setItem(this.newTodoItem, this.newTodoItem);    // 로컬 스토리지에 저장
        },
        clearInput: function(){
            this.newTodoItem = '';     // 저장 후 input box 초기화
        },
    }
}
</script>

<style>

</style>
