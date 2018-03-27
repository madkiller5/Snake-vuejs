<template>
 <div>
   <p id="nickname">Nickname: {{nickname}}</p>
   <p id="score">Life: {{score}}</p>
 </div>
</template>

<script>
import { bus } from '../main';

export default {
  props: ['nickname'],
  data () {
    return {
      nickname: 'Patryk',
      score: 100
    }
  },
  methods: {
    isAlive: function(){
      if(this.score>0)
        bus.$emit('Alive',true);
      else
        bus.$emit('Alive',false);
    }
  },
  created() {
    bus.$on('getScore',(data)=>{
      this.score+=data;
      this.isAlive();
    });
  }
}
</script>

<style scoped>
p{
  font-family: 'Caveat', cursive;
  margin: 10px;
  font-size: 20px;
  display: inline-block;
}
#nickname{
  float: left;
}
#score{
  position: relative;
  right: 0;
  margin-left: auto;
  margin-right: 50px;
}
div{
  display: inline-flex;
  width: 500px;
  box-sizing: border-box;
  border-top: 2px dotted gray;
}
</style>
