<template>
  <div class="overlay-dialog">
    <audio ref="selectsound" preload="auto" src="@/assets/sound/select.mp3" muted></audio>
    <audio ref="validationsound" preload="auto" src="@/assets/sound/dialogBoxVictoire.mp3" muted></audio>
    <div class="dialog">
      <p>{{ memos[dialogId].text_dialog }}</p>
      <img  class="trophe" src="@/assets/var_icons/trophe.gif" alt="">
      <div class="button-container">
        <button>
          <router-link :to="{ name: 'Memo', params: {step: dialogId }}">Voir l'étape «{{ memos[dialogId].btn_step }}»</router-link>
        </button>
        <button v-on:click="()=>{registerClick();}">{{ memos[dialogId].btn_dialog }}</button>
      </div>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import VueConfetti from 'vue-confetti'
 
  Vue.use(VueConfetti)

  export default {
    name: 'DialogBox',
    props: ['memos','dialogId'],
    mounted (){
      this.start();
      this.playValidationSound();
    },
    methods: {
      start() {
        this.$confetti.start({
          particles: [
            {
              type: 'rect',
              size: 8,
            },
          ],
          particlesPerFrame: 2,
          defaultDropRate: 15,
          windSpeedMax: 2,
        });
        window.setTimeout(() => {this.stop();window.setTimeout(()=>{const conf = document.getElementById("confetti-canvas");conf.remove();},4000)},1500)
      },
      stop() {
        this.$confetti.stop();
      },
      registerClick (){
        this.playSelect();
        window.setTimeout(() => {this.$emit('hideDialogBox')},250);
      },
      playSelect(){
        let audio = this.$refs.selectsound;
        audio.volume = 0.1;
        audio.play(); 
      },
      playValidationSound(){
        let audio = this.$refs.validationsound;
        audio.volume = 0.1;
        audio.play(); 
      }
    }
  }
</script>


<style scoped>
@keyframes overlay {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
}

.overlay-dialog {
  position: absolute;
  top: 0;
  z-index: 100;
  height: 100%;
  width: 100%;
  background: rgba(0,0,0,0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  animation: overlay 0.5s ease-out;
}

.dialog {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  background-color:#D5ffcf;
  height: 545px;
  width: 320px;
}

p {
  margin: 0;
  font-size: 22px;
  width: 290px;
  color: #4D4D4D;
  letter-spacing: 0;
  text-align: center;
}

@keyframes vibrate-fast {
  0%{
    -webkit-transform: translate(0);
    transform: translate(0);
  }
  10%{
    -webkit-transform: translate(-2px,-2px);
    transform: translate(-2px,-2px);
  }
  20%{
    -webkit-transform: translate(2px,-2px);
    transform: translate(2px,-2px);
  }
  30%{
    -webkit-transform: translate(-2px,2px);
    transform: translate(-2px,2px);
  }
  40%{
    -webkit-transform: translate(2px,2px);
    transform: translate(2px,2px);
  }
  50%{
    -webkit-transform: translate(-2px,-2px);
    transform: translate(-2px,-2px);
  }
  60%{
    -webkit-transform: translate(2px,-2px);
    transform: translate(2px,-2px);
  }
  70%{
    -webkit-transform: translate(-2px,2px);
    transform: translate(-2px,2px);
  }
  80%{
    -webkit-transform: translate(-2px,-2px);
    transform: translate(-2px,-2px);
  }
  90%{
    -webkit-transform: translate(2px,-2px);
    transform: translate(2px,-2px);
  }
  100%{
    -webkit-transform: translate(0);
    transform: translate(0);
  }
}

.trophe {
  width: 60%;
  height: auto;
  image-rendering: pixelated;
  animation: vibrate-fast 1s ease-out 2 normal forwards;
  filter: drop-shadow(3px 3px 0px rgba(34, 34, 34, 0.3));
}

.button-container {
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: flex-end;
}

button {
  outline: none;
  border: none;
  padding: 10px;
  width: 120px;
  height: 80px;
  font-size: 20px;
  background: #FFFFFF;
  box-shadow: 3px 3px 0 0 rgba(0,0,0,0.50);
  display: inline-block;
  word-wrap: break-word;
  transition: all 0.2s;
}

button:hover {
  cursor: url('/hand.png'), auto;
}

@media (hover: hover) and (pointer: fine) {
  button:hover, button:active {
  background: #FFFFFF;
  box-shadow: none;
  transform: scale(1.05);
  transform: translate(1px,1px);
  }
}

a {
  color: black;
  text-decoration: none;
}

</style>
