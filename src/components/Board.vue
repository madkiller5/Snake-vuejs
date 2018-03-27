<template>
  <div id="board">

    <div  v-for="(index,item) in items" class="tiles">
      <Tile :value="item">
      </Tile>
    </div>

  </div>
</template>

<script>
import Tile from './Tile';
import { bus } from '../main';

export default {
  components: {
    Tile
  },
  data () {
    return {
      snake: [
      {
        posx:0,
        posy:0
      },
      {
        posx:0,
        posy:1
      },
      {
        posx:0,
        posy:2
      }],
      snakeHeadId: 2,
      snakeHeadPos: {
        posx:0,
        posy:2,
      },
      items: [],
      direction: 1,
      snakeLength: 3,
      gameOvers: false,
      intervalID: 0,
      flag: false
    }
  },
  methods: {
    placeSnack: function(){
      while(1){
        var position = Math.floor(Math.random() * 2000);
        if(this.getColor(position)!="black"){
          this.colorize(position,"brown");
          break;
        }
      }
    },
    getColor: function(id){
      var tiles = document.getElementsByClassName('tile');
      return tiles[id].style.backgroundColor;

    },

    colorize: function(id,color){
      var tiles = document.getElementsByClassName('tile');
      tiles[id].style.backgroundColor = color;
    },
    actualize: function(){
      switch(this.direction){
        case 1:
          this.snakeHeadPos.posy++;
          break;
        case 2:
          this.snakeHeadPos.posx++;
          break;
        case 3:
          this.snakeHeadPos.posy--;
          break;
        case 4:
          this.snakeHeadPos.posx--;
          break;
      };
      if(this.snakeHeadPos.posy >= 50){
        this.snakeHeadPos.posy = 0;
      }
      else if( this.snakeHeadPos.posx < 0){
        this.snakeHeadPos.posx=39;
      }
      else if( this.snakeHeadPos.posy < 0){
        this.snakeHeadPos.posy = 49;
      }
      else if( this.snakeHeadPos.posx >= 40){
        this.snakeHeadPos.posx = 0;
      }
      this.snake.push({
        posx: this.snakeHeadPos.posx,
        posy: this.snakeHeadPos.posy
      });
      this.snakeHeadId = this.snakeHeadPos.posx*50 + this.snakeHeadPos.posy;
      if(this.getColor(this.snakeHeadId)=="black"){
        //gameover
        this.gameOver();
      }
      else if(this.getColor(this.snakeHeadId)!='brown'){
        this.score(-1);
        var z = this.snake.shift();
        this.colorize(z.posx*50+z.posy,"white");
      }
      else{
        this.score(50);
        this.placeSnack();
      }
      this.colorize(this.snakeHeadId,"black");
      this.flag=false;
      
    },
    gameOver: function(){
      clearInterval(this.intervalID);
      this.$emit('gameOver',true);
    },
    score: function(q){
      bus.$emit('getScore',q);
    }
  },
  beforeMount() {
    this.items.length = 2000;
    
  },
  mounted(){
    this.colorize(0,"black");
    this.colorize(1,"black");
    this.colorize(2,"black");
    this.placeSnack();
    
    this.intervalID = setInterval(()=>{
      this.actualize();
      },150);

    document.addEventListener('keydown', (event)=>{
      var keyName = event.key;
      if(!this.flag){
        switch(keyName){
        case "ArrowUp":
          if(this.direction!=2)
          this.direction = 4;
          break;
        case "ArrowDown":
          if(this.direction!=4)
          this.direction = 2;
          break;
        case "ArrowRight":
          if(this.direction!=3)
          this.direction = 1;
          break;
        case "ArrowLeft":
          if(this.direction!=1)
          this.direction = 3;
          break;

        }
        this.flag=true;

      }
      
    });
  },
  created(){
    bus.$on('StopGame',(data)=>{
      if(data==true){
        clearInterval(this.intervalID);
      }
    })
  }
}
</script>

<style scoped>
p{
  font-size:20px;
}
#board{
  width: 500px;
  height: 500px;
  clear: both;
}
.tile{
  background-color: white;
  box-sizing: border-box;
  margin: 0;
  display: inline-block;
  width: 10px;
  height: 10px;
  border: 1px dotted gray;
}

div{
  display: inline;
}
</style>
