<template>
    <div class="wrapper clearfix">
        <how-to-play
          v-bind:isOpenPopup="isOpenPopup"
          v-on:handleConfirmEvent="handleConfirm"
        />
        <players
          v-bind:isWinner="isWinner"
          v-bind:scorePlayers='scorePlayers'
          v-bind:currentScore='currentScore'
          v-bind:activePlayer='activePlayer'
        />
        
        <control
          v-bind:isPlaying="isPlaying"
          v-on:startGameEvent='startGame'
          v-on:handleRollDices='handleRollDices'
          v-on:handleHoldScore="handleHoldScore"
          v-bind:scoreToWin="scoreToWin"
          v-on:handleChangeScoreToWin='handleChangeScoreToWin'
        />
        

        <dices
          v-bind:dices="dices"
        />
    </div>
</template>

<script>
import Players from './components/Players.vue';
import Control from './components/Control.vue';
import Dices from './components/Dices.vue';
import HowToPlay from './components/HowToPlay.vue'

export default {
  components: { Players, Control, Dices, HowToPlay },
  name: 'app',
  data () {
    return {
      isPlaying: false,
      isOpenPopup: false,
      activePlayer: 0,
      scorePlayers: [0, 0],
      currentScore: 3,
      dices: [2, 5],
      scoreToWin: 100
    }
  },
  computed: {
    isWinner() {
      let { scorePlayers, scoreToWin } = this;
      if ( scorePlayers[0] >= scoreToWin || scorePlayers[1] >= scoreToWin ) {
        this.isPlaying = false;
        return true;
      } else {
        return false
      }
    }
  },

  methods: {
    handleChangeScoreToWin(e) {
      let number = parseInt(e.target.value)
      if (!isNaN(number)) {
        this.scoreToWin = parseInt(e.target.value);
      } else {
        this.scoreToWin = ''
      }
    },

    nextPlayer() {
      this.activePlayer = this.activePlayer == 0 ? 1 : 0;
      this.currentScore = 0;
    },

    startGame() {
      this.isOpenPopup = true;
    },

    handleConfirm() {
      this.isOpenPopup = false;
      this.isPlaying = true;
      this.dices = [1, 1];
      this.activePlayer = 0;
      this.scorePlayers = [0, 0];
      this.currentScore = 0 
    },

    handleRollDices() {
      if(this.isPlaying) {
        let dice1 = Math.floor(Math.random() * 6) + 1;
        let dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];

        if(dice1 == 1 || dice2 == 1) {
          setTimeout(() => {
            alert('Too Bad, Turn Skip !')
          }, 10)
          this.nextPlayer()
        } else {
          this.currentScore = this.currentScore + dice2 + dice1
        }
      } else {
        alert('Pls click "New Game" button')
      }
    },
    handleHoldScore() {
      if(this.isPlaying) {
        // đỡ phải khai báo nhiều this
        let { scorePlayers, activePlayer, currentScore } = this;
        // lấy điểm số cũ
        let oldScore = scorePlayers[activePlayer];
        // tạo ra 1 bản sao
        let cloneScorePlayer = [...scorePlayers];
        // cộng dồn điểm cũ và điểm mới
        cloneScorePlayer[activePlayer] = oldScore + currentScore;
        // gắn scorePlayer = bản sao để rerender lại, và update data
        this.scorePlayers = cloneScorePlayer;
        // hoặc dùng hàng $set: this.$set(this.scorePlayer, activePlayer, oldScore + currentPlayer)
        if(!this.isWinner) {
          this.nextPlayer();
        }
      } else {
        alert('Pls click "New Game" button')
      }
    }
  }
}
</script>
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('./back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}





#dice-1 { top: 120px; }
#dice-2 { top: 250px; }

.dice {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 120px;
    height: 120px;
    box-shadow: 0px 10px 60px rgba(0, 0, 0, 0.10);
}
.spinner div {
    position: absolute;
    width: 120px;
    height: 120px;
    border: 1px solid #ccc;
    box-shadow: 0 0 20px rgba(0,0,0,0.2);
    text-align: center;
    line-height: 120px;
    font-size: 100px;
    color: #42b983;
    font-size: 0;
    transition: all .3s ease;
    opacity: 1;
}

.spinner .face1 { 
    -webkit-transform: translateZ(60px);
    -ms-transform: translateZ(60px);
    transform: translateZ(60px);
    background-image: url("dice-1.png");
    background-position: center;
    background-size: cover;
}
.spinner .face2 { 
    -webkit-transform: rotateY(90deg) translateZ(60px); 
    -ms-transform: rotateY(90deg) translateZ(60px); 
    transform: rotateY(90deg) translateZ(60px); 
    background-image: url("dice-2.png");
    background-position: center;
    background-size: cover;
}
.spinner .face3 { 
    -webkit-transform: rotateY(90deg) rotateX(90deg) translateZ(60px); 
    -ms-transform: rotateY(90deg) rotateX(90deg) translateZ(60px); 
    transform: rotateY(90deg) rotateX(90deg) translateZ(60px); 
    background-image: url("dice-3.png");
    background-position: center;
    background-size: cover;
}
.spinner .face4 { 
    -webkit-transform: rotateY(180deg) rotateZ(90deg) translateZ(60px); 
    -ms-transform: rotateY(180deg) rotateZ(90deg) translateZ(60px); 
    transform: rotateY(180deg) rotateZ(90deg) translateZ(60px); 
    background-image: url("dice-4.png");
    background-position: center;
    background-size: cover;
}
.spinner .face5 { 
    -webkit-transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px); 
    -ms-transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px); 
    transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px); 
    background-image: url("dice-5.png");
    background-position: center;
    background-size: cover;
}
.spinner .face6 { 
    -webkit-transform: rotateX(-90deg) translateZ(60px); 
    -ms-transform: rotateX(-90deg) translateZ(60px); 
    transform: rotateX(-90deg) translateZ(60px); 
    background-image: url("dice-6.png");
    background-position: center;
    background-size: cover;
}

.spinner {
    -webkit-transform-style: preserve-3d;
    -ms-transform-style: preserve-3d;
    transform-style: preserve-3d;
    -webkit-transform-origin: 60px 60px 0;
    -ms-transform-origin: 60px 60px 0;
    transform-origin: 60px 60px 0;
    -webkit-transition: all .9s ease;
    -o-transition: all .9s ease;
    transition: all .9s ease;
    -webkit-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
    -ms-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
    -o-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
	transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
}
.spinner.dice-1 {
	-webkit-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
    -ms-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
    -o-transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
    transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg);
}
.spinner.dice-2 {
	-webkit-transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg);
    -ms-transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg);
    -o-transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg);
    transform: rotateX(0deg) rotateY(-90deg) rotateZ(0deg); 
}
.spinner.dice-3 {
    -webkit-transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg);
    -ms-transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg);
    -o-transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg);
    transform: rotateX(0deg) rotateY(-90deg) rotateZ(90deg); 
}
.spinner.dice-4 {
    -webkit-transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg);
    -ms-transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg);
    -o-transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg);
    transform: rotateX(0deg) rotateY(180deg) rotateZ(90deg); 
}
.spinner.dice-5 {
    -webkit-transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg);
    -ms-transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg);
    -o-transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg);
    transform: rotateX(0deg) rotateY(90deg) rotateZ(0deg); 
}
.spinner.dice-6 {
    -webkit-transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg);
    -ms-transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg);
    -o-transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg);
    transform: rotateX(90deg) rotateY(90deg) rotateZ(0deg); 
}

</style>
