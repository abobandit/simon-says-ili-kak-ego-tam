<template>
  <div id="app">
<!--    <HelloWorld/>-->
    <GameMain/>
  </div>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue";
import GameMain from "@/components/GameMain.vue";

export default {
  name:"App",
  components: {GameMain, HelloWorld},
};
</script>

<style scoped>
.simon-board {
  display: flex;
  flex-wrap: wrap;
  width: 300px;
}

.simon-button {
  width: 100px;
  height: 100px;
  margin: 5px;
  cursor: pointer;
  opacity: 0.6;
}

.simon-button.active {
  opacity: 1;
  transition: opacity 0.3s ease-in-out;
}

.blue {
  background-color: blue;
}

.red {
  background-color: red;
}

.green {
  background-color: green;
}

.yellow {
  background-color: yellow;
}
</style>
