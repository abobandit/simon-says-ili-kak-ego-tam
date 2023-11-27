<template>
  <div>
    <div class="simon-board">
      <div
          v-for="(button, index) in buttons"
          :key="button"
          :ref="'button' + button"
          class="simon-button"
          :style="getColor(button)"
          :class="{ active: activeButton === button }"
          @click="buttonClicked(button)"
      />
    </div>
    <div>
      <button @click="startGame">Start Game</button>
      <p>Level: {{ level }}</p>
      <p>Status: {{ gameStatus }}</p>
    </div>
    <div>
      Выбрать сложность
      <div v-for="(value,key) in difficulty">
        {{key}}
        <input type="radio"
               :disabled="gameStatus!=='Not started'"
               :value="value"
               name="difficulty"
               v-model="selectedDifficulty"
               class="difficulty">
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name:"GameMain",
  data() {
    return {
      buttons: [0, 1, 2, 3],
      sequence: [],
      playerSequence: [],
      activeButton: null,
      level: 1,
      selectedDifficulty:1000,
      gameStatus: 'Not started',
      difficulty: {
        'легкий': 1500,
        'нормальный': 1000,
        'сложный': 400
      }
    };
  },
  methods: {
    getColor(index) {
      switch (index) {
        case 0:
          return 'background: blue';
        case 1:
          return 'background: red';
        case 2:
          return 'background: green';
        case 3:
          return 'background: yellow';
        default:
          return '';
      }
    },
    buttonClicked(index) {
      if (this.gameStatus!=='isPlaying'){
        this.activeButton = index;
        this.playSound(index);
        setTimeout(() => {
          this.activeButton = null;
          console.log('awaiting button clicked')
        }, 300);
        this.playerSequence.push(index);
        this.checkSequence();
      }
    },
    playSound(index) {
      const audio = new Audio(require(`@/assets/sounds/${index+1}.mp3`)); // Звуковые файлы от 1 до 4.mp3
      audio.play();
    },
    async startGame() {
      this.sequence = [];
      console.log('стерт')
      await this.handleSequence()
    },
    async handleSequence(){
      this.gameStatus = 'Playing';
      this.playerSequence = [];
      //мы нашли пидораса, который нам все руинит, когда вернешься, исправь его, а то у тебя очередь дважды заполняется
      for (let i = this.sequence.length; i < this.level; i++) {
        const randomIndex = Math.floor(Math.random() * this.buttons.length);
        this.sequence.push(this.buttons[randomIndex]);
        console.log('вот тут пидорас')
        await this.playSequence(i);
      }
      this.gameStatus = 'Listening';
    },
    playSequence(i) {
      return new Promise((resolve) => {
        setTimeout(() => {
          this.activeButton = this.sequence[i];
          console.log(this.sequence,'sequence')
          this.playSound(this.sequence[i]);
          setTimeout(() => {
            this.activeButton = null;
            resolve();
          }, this.selectedDifficulty);
        }, this.selectedDifficulty);
      })
    },
    checkSequence() {
      for (let i = 0; i < this.playerSequence.length; i++) {
        if (this.playerSequence[i] !== this.sequence[i]) {
          this.level = 1
          this.sequence = []
          this.playerSequence= []
          this.gameStatus = 'Game Over';
          return;
        }
      }
      if (this.playerSequence.length === this.sequence.length) {
        this.level++;
        this.gameStatus = 'Next Level';
        setTimeout(() => {
          this.handleSequence();
        }, 1000);
      }
    }
  }
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
  opacity: 0.5;
}

.simon-button.active {
  opacity: 1;
  box-shadow: 1px  2px black;
  transition: opacity 0.3s ease-in-out;
}
</style>