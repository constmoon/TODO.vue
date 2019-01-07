<template>
    <div class="inputBox shadow">
        <!-- v-model="특정 데이터 속성": input에 입력된 텍스트값을 동적으로 vue instance에 맵핑 -->
        <input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo">
        <!-- <button v-on:click="addTodo">add</button> -->
        <span class="addContainer" v-on:click="addTodo">
            <i class="fas fa-plus addBtn"></i>
        </span>
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

<style scoped>
input:focus{
    outline: none;
}
.inputBox{
    background: #fff;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
}
.inputBox input{
    border-style: none;
    font-size: 0.9rem;
}
.addContainer{
    float: right;
    background: linear-gradient(to right, #6478FB, #8763FB);
    display: block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
}
.addBtn{
    color: #fff;
    vertical-align: middle;
}

</style>
