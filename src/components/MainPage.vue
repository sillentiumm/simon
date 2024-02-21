<template>
  <div class="main">
    <h2>Simon says</h2>
    <div class="game">
      <div class="game-field" :class="{disable: this.enableAction == false}"> 
        <div @click="turnPlayer(0)" class="section section0" :class="{sectionActive: this.currentNumber === 0}">
        </div>
        <div @click="turnPlayer(1)" class="section section1" :class="{sectionActive: this.currentNumber === 1}"></div>
        <div @click="turnPlayer(2)" class="section section2" :class="{sectionActive: this.currentNumber === 2}"></div>
        <div @click="turnPlayer(3)" class="section section3" :class="{sectionActive: this.currentNumber === 3}"></div>
      </div>
      <div class="game-info">
        <div>
          Round: {{ roundNumber }}
        </div>
        <button @click="startGame" class="start-btn">
          start 
        </button>
        <div class="difficult" :class="{disable: this.enableDifficult == false}">
          <button @click="changeDifficult(0)" class="difficult-btn" :class="{difficultActive: this.difficult === 0}">
            easy
          </button>
          <button @click="changeDifficult(1)" class="difficult-btn" :class="{difficultActive: this.difficult === 1}">
            medium
          </button>
          <button @click="changeDifficult(2)" class="difficult-btn" :class="{difficultActive: this.difficult === 2}">
            hard
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import sound0 from '../assets/sounds/sound1.mp3'
import sound1 from '../assets/sounds/sound2.mp3'
import sound2 from '../assets/sounds/sound3.mp3'
import sound3 from '../assets/sounds/sound4.mp3'

export default {
  setup() {
    const audio0 = new Audio(sound0);
    const audio1 = new Audio(sound1);
    const audio2 = new Audio(sound2);
    const audio3 = new Audio(sound3);
    
    function play0() {
      audio0.pause();
      audio0.currentTime = 0;
      audio0.play()
    }
    function play1() {
      audio1.pause();
      audio1.currentTime = 0;
      audio1.play()
    }
    function play2() {
      audio2.pause();
      audio2.currentTime = 0;
      audio2.play()
    }
    function play3() {
      audio3.pause();
      audio3.currentTime = 0;
      audio3.play()
    }

    return { play0, play1, play2, play3 }
  },
  data() {
    return {
      arr: [],
      dublicate: [],
      currentNumber: null,
      enableAction: false,
      enableDifficult: true,
      difficult: 1,
      section0Active: false,
      section1Active: false,
      section2Active: false,
      section3Active: false,
    }
  },
  methods: {
    startGame() {
      this.reset()
      this.enableDifficult = false
      this.turn()
    },
    changeDifficult(dif) {
      this.difficult = dif
    },
    turn() {
      this.arr.push(this.randomizer())
      this.turnAnimate()
      this.enableAction = false
      this.dublicate = [...this.arr];
    },

    turnAnimate() {
      setTimeout(() => {
        for ( let i = 0; i < this.arr.length; i++) {   
          setTimeout(() => {
            setTimeout(() => {
              this.currentNumber = this.arr[i]
              if(this.arr[i]==0) { this.play0() }
              else if(this.arr[i]==1) { this.play1() }
              else if(this.arr[i]==2) { this.play2() }
              else { this.play3() }
            },this.difficultCoefficient/4)
            setTimeout(() => {
              this.currentNumber = null
            },this.difficultCoefficient/3*2)
          }, i*this.difficultCoefficient);    
        }

        setTimeout (() => {
          this.enableAction = true
        }, this.arr.length * this.difficultCoefficient)
      }, this.difficultCoefficient/2)
    },

    turnPlayer(idx) {
      if(idx == 0) { this.play0() }
      else if(idx == 1) { this.play1() }
      else if(idx == 2) { this.play2() }
      else if(idx == 3) { this.play3() }

      if(idx == this.dublicate[0]) {
        this.dublicate.shift()
        if(!this.dublicate.length) {
          this.turn()
        }
      } else {
        this.reset()
      }
    },
    randomizer() {
      return Math.floor(Math.random() * 4);
    },
    reset() {
      this.arr = []
      this.dublicate = []
      this.currentNumber = null
      this.enableAction = false
      this.enableDifficult = true
    }
  },
  computed: {
    roundNumber() {
      return this.arr.length
    },
    difficultCoefficient() {
      if(this.difficult == 0 ) return 1500
      else if(this.difficult == 1 ) return 1000
      else return 400
    }
  }
}
</script>

<style>
.main {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-size: 20px;
}
.game {
  display: flex;
  justify-content: center;
  align-items: center;
}
.game-field {
  width: 320px;
  height: 320px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  border: none;
  border-radius: 100%;
  background-color: lightcyan;
  margin-top: 24px;
  margin-right: 24px;
}
.game-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.disable {
  pointer-events: none;
}
.start-btn {
  font-size: 18px;
  width: 120px;
  height: 40px;
  background-color: lightblue;
  border: none;
  border-radius: 8px;
  margin-top: 24px;
  cursor: pointer;
}
.start-btn:active {
  border: 1px solid gray;
}
.section {
  width: 159.5px;
  height: 159.5px;
  border: none;
  opacity: .6;
  transition: .1s;
}
.sectionActive {
  opacity: .9;
}
.section:active {
  opacity: .9;
}
.section0 {
  background-color: blue;
  border-radius: 100% 0 0 0;
}
.section1 {
  background-color: #ff0000;
  border-radius: 0 100% 0 0;
}
.section2 {
  background-color: yellow;
  border-radius: 0 0 0 100%;
}
.section3 {
  background-color: green;
  border-radius: 0 0 100% 0;
}

.difficult {
  display: flex;
  flex-direction: column;;
  margin-top: 24px;
}
.difficult-btn {
  background-color: lightblue;
  width: 80px;
  height: 30px;
  border: none;
  border-radius: 8px;
  margin-top: 8px;
  cursor: pointer;
}
.difficultActive {
  border: 1px solid gray;
}

</style>
