<template>
  <div>
      <ul>
          <!-- v-bind:key => todoItem의 텍스트가 key가 되어 중복 X, v-for의 성능 가속화 -->
          <li v-for="todoItem in todoItems" v-bind:key="todoItem">
              {{ todoItem }}
          </li>
          
      </ul>
  </div>
</template>

<script>
export default {
    // 로컬 스토리지에서 꺼내서 담을 데이터
    data: function(){
        return{
            todoItems: []
        }
    },
    // 인스턴스가 생성되자마자 호출됨
    created: function(){
        if(localStorage.length > 0){
            for(var i=0; i<localStorage.length; i++){
                // webpack dev server를 제외한 나머지들 저장
                if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
                    // this로 같은 인스턴스의 todoItems에 접근
                    this.todoItems.push(localStorage.key(i));
                }
            }
        }
    }
};
</script>

<style>
</style>
