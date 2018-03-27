<template>
  <div id="gameover">
    <p>You Lost!</p>
    <button v-on:click="tryAgain">Try Again!</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      
    }
  },
  methods: {
    tryAgain: function(){
      window.location.reload();
    }
  }
}
</script>

<style scoped>
#gameover{
  background-color: lemonchiffon;
  font-family: 'Caveat', cursive;
  margin: 0 auto;
  box-sizing: border-box;
  width: 500px;
  height: 300px;
  padding: 100px;
}
#gameover p{
  font-size: 30px;
}
</style>
