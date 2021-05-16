<template>
    <div class="wrapper clearfix">
        <Players 
          v-bind:isWinner="isWinner"
          v-bind:scoresPlayer="scoresPlayer"
          v-bind:activePlayer="activePlayer"
          v-bind:currentScore="currentScore"
        />
        <Controls
        v-bind:isPlaying="isPlaying"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        v-bind:finalScore="finalScore"
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:hnadleHoldScore="hnadleHoldScore"
        />
        <Dices v-bind:dices="dices"/>
        <popup-rule 
          v-bind:isOpenPopup="isOpenPopup" 
          v-on:handleConfirm="handleConfirm"
        />
    </div>
</template>

<script>
import Players from './components/Players';
import Controls from './components/Controls';
import Dices from './components/Dices';
import PopupRule from './components/PopupRule';

export default {
  name: "App",
  data () {
      return {
        isPlaying: false,
        isOpenPopup: false,
        activePlayer: 0,
        scoresPlayer: [23, 30],
        currentScore:0,
        dices: [5,5],
        finalScore: 10
      }
  },
  components: {
      Players,
      Controls,
      Dices,
      PopupRule
  },
  computed: {
    isWinner() {
      let { scoresPlayer , finalScore  } = this;
      if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore){
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.isPlaying=false ;
        return true
        
      }
      return false;
    }
  },
  methods: {
    handleChangeFinalScore(e) {
      var number = parseInt(e.target.value);
      if(isNaN(number)) {
        this.finalScore = '';
      }else {
        this.finalScore = number;
      }
    },
    handleNewGame() {
      this.isOpenPopup = true;
    },
    handleConfirm() {
      this.isPlaying =true;
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scoresPlayer = [0,0];
      this.currentScore = 0;
      this.dices = [1,1];
    },
    nextPlayer(){
      this.activePlayer = this.activePlayer === 0 ? 1: 0;
      this.currentScore =0;
    },
    handleRollDice() {
      if(this.isPlaying) {
        var dice1 = Math.floor(Math.random()* 6) +1;
        var dice2 = Math.floor(Math.random()* 6) +1;
        this.dices= [dice1,dice2];
        console.log(dice1,dice2);
        if(dice1 === 1 || dice2 ===1){
          let activePlayer = this.activePlayer;
          setTimeout(function () {
            alert(`Nguoi choi Player ${activePlayer} đã quay trúng số 1. Rất tiếc`)
          
          },10)
          if(!this.isWinner){
            this.nextPlayer();
          }
        }else{
          this.currentScore = this.currentScore +dice1 + dice2;
        }
      }else{
        alert('Vui long nhất vào nút NewGame');
      }
    },
    hnadleHoldScore() {
      if(this.isPlaying) {
        let { scoresPlayer ,activePlayer ,currentScore} = this;
        let scoreOld = scoresPlayer[activePlayer];
        let cloneScorePlayer = [...scoresPlayer];
            cloneScorePlayer[activePlayer] = scoreOld + currentScore;
        this.scoresPlayer = cloneScorePlayer;
        if(!this.isWinner){
          this.nextPlayer(); 
        }   
      }else {
         alert('Vui long nhất vào nút NewGame');
      }
    }
  }
};
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
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("../public/assets/back.jpg");
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

</style>
