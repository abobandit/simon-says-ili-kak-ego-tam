<template>
  <div @click="toggleActive" ref="element" class="circleElement"/>
</template>
<script>
export default {
  name: 'CircleElement',
  props:{
    bg:String,
    isPlaying:Boolean,
    position: {
      type:Number,
      validator: function (value) {
        return [1,2,3,4].filter(num => num===value).length
      }
    },
  },
  methods:{
    assignStyling(){
      const element = this.$refs.element
      element.style.background = this.bg
      if (this.position === 4){
        element.style.transform = 'rotate(90deg)'
      }
      if (this.position === 3){
        element.style.transform = 'rotate(180deg)'
      }
      if (this.position === 1){
        element.style.transform = 'rotate(-90deg)'
      }
    },
    toggleActive() {
      this.$emit('update:isActive', !this.isPlaying);
    },
    playSound() {
      const audio = new Audio(require(`@/assets/sounds/${this.position}.mp3`)); // Замените путь на ваш файл со звуком
      audio.play();
    }
  },
  mounted() {
      this.assignStyling()
  },
  watch:{
    isPlaying(newVal){
      if (newVal){
        this.playSound()
      }
    }
  }
}
</script>
<style scoped>
.circleElement {
  width: 296px;
  border-radius: 0 999px 0 0;
  height: 296px;
  opacity: .6;
}
.circleElement:hover{
 opacity: 1;
}
</style>