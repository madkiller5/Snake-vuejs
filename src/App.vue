<template>
  <div id="app">
    <Hello/> 
    <Panel v-bind:nickname="nickname" />
    <Board v-if="show && !gameOver" v-on:gameOver="updateStatus($event)"/>
    <GameOver v-if="gameOver"/>

    <div id="start" v-if="!show && !gameOver">
      <label>Enter your name:</label>
      <input v-model="nickname" placeholder="enter your nick..." maxlength="30"/>
      <button v-on:click="validate">Play a Game</button>
      <p v-if="notValid">You must enter your name!</p>
    </div>

    <div id="footer">
      <p>Created in 2018 by patryk.skrzyniarz@gmail.com</p>
    </div>
    
  </div>
</template>

<script>
import GameOver from './components/GameOver';
import Panel from './components/Panel';
import Hello from './components/Hello';
import Board from './components/Board';
import { bus } from './main';


export default {
  name: 'App',
  components: {
    Panel,
    GameOver,
    Hello,
    Board
  },
  data() {
    return {
      nickname: '',
      show: false,
      gameOver: false,
      notValid: false
    }
  },
  methods: {
    updateStatus: function(updatedStatus){
      this.gameOver = updatedStatus;
    },
    
    validate: function(){
        if(this.nickname==''){
          this.notValid=true;
        }
        else{
          this.show=!this.show;
        }
    }
  },
  created(){
    bus.$on('Alive',(data)=>{
      this.gameOver=!data;
      if(this.gameOver==true)
        bus.$emit('StopGame',true);
      else
        bus.$emit('StopGame',false);
    });
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Caveat|Roboto');
body{
  background: radial-gradient(lightcyan ,rgb(212, 212, 255));
}
p{
  font-size: 20px;
}
*{
    margin: 0;
    padding: 0;
    font-size: 0px;
}
label, input, button{
  margin: 5px;
  font-size: 20px;
}
#app {
  background-color: lightgreen;
  box-shadow: 15px 15px rgb(58, 99, 58);
  margin: 0 auto;
  margin-top: 50px;
  padding: 0;
  padding-top: 5px;
  width: 500px;
  text-align: center;
}
#start{
  font-family: 'Caveat', cursive;
  padding: 20px;
  background: floralwhite;
  width: 500px;
  height: 300px;
  box-sizing: border-box;
  border-top:dotted 2px gray;
}
#start p{
  font-size:30px;
  color: green;
}
#footer{
  box-sizing: border-box;
  background-color: lightgreen;
  border-top: 2px dotted gray;
  
}
#footer p{
  font-family: 'Caveat', cursive;
  font-size: 14px;
}

</style>
