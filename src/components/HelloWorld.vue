<template>
 <div class="container">
   <div class="circleContainer">
     <CircleElement bg="red" :position="1"/>
     <CircleElement bg="blue" :position="2"/>
     <CircleElement bg="yellow" :position="3"/>
     <CircleElement bg="green" :position="4"/>
   </div>
 </div>
</template>

<script>
import CircleElement from "@/components/CircleElement.vue";

export default {
  name: 'HelloWorld',
  components: {CircleElement},
  data(){
    return {
      playerSequence:[],
      sequence: [],
      gameStatus:''
    }
  },
  methods: {
    async startGame() {
      this.gameStatus = 'Playing';
      this.sequence = [];
      this.playerSequence = [];
      for (let i = 0; i < this.level; i++) {
        const randomIndex = Math.floor(Math.random() * this.buttons.length);
        this.sequence.push(this.buttons[randomIndex]);
        await this.playSequence(i);
      }
      this.gameStatus = 'Listening';
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.circleContainer{
  display: grid;
  width: min-content;
  grid-template-rows: repeat(2,1fr);
  grid-template-columns: repeat(2,1fr);
}
</style>
