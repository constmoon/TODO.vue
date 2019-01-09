<template>
    <div class="clearAllContainer">
        <span class="clearAllBtn" v-on:click="clearTodo">Clear All</span>
    </div>
</template>

<script>
export default {
    methods:{
        clearTodo: function(){
            localStorage.clear();
        }
    }
}
</script>

<style scoped>
.clearAllContainer{
    width: 8.5rem;
    height: 50px;
    line-height: 50px;
    background-color: #fff;
    border-radius: 5px;
    margin: 0 auto;
    box-shadow: 0.1rem 0.1rem 0.5rem rgba(127,127,127,0.7);
}
.clearAllBtn{
    color: #e20303;
    display: block;
}
</style>
