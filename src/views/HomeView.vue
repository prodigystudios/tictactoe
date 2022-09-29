<template>
  <div v-if="gameEnded" class="score-screen">
    <h2 class="endgame-name">{{winner}} Har vunnit spelet! GRATTIS </h2>
  </div>

  <!-- Start game screen -->
  <transition name="up">
    <div class="main-container" v-if="!animStarted">
      <h1>TIC TAC TOE</h1>
      <div class="selected-mode-container">
        <label>Välj spel läge!</label>
        <select v-model="playMode" @change="PlayModeSelected()">
          <option value="2 players">2 SPELARE</option>
          <option value="Ai">A.I</option>
        </select>
      </div>


      <!-- if ai mode is selected we show this -->
      <section v-if="aiMode">
        <div class="player-name-container" v-for="player in players" :key="player.id">
          <section class="name-container-section" v-if="player.id === 1">
            <label for players1Name>Spelare {{player.id}}</label>
            <input type="text" placeholder="Namn" v-model="player.name">
          </section>
        </div>
      </section>

      <!-- if 2 player mode is selected we show this -->
      <section v-if="twoPlayerMode">
        <div class="player-name-container" v-for="player in players" :key="player.id">
          <section class="name-container-section">
            <label for players1Name>Spelare {{player.id}}</label>
            <input type="text" placeholder="Namn" v-model="player.name">
          </section>
        </div>
      </section>

      <button @click="StartGame()" class="startgame-btn">Spela</button>
    </div>
  </transition>

  <!-- Gameplay Bord -->
  <transition name="up">
    <div v-if="gameStarted" class="wrapper">
      <div class="playing-field">
        <div v-for="player in players" :key="player.id">
          <h2 v-if="player.isPlaying">Spelare: {{player.name}}</h2>
        </div>
        <div class="row">
          <div v-for="button in buttons" :key="button.id" class="box">
            <button class="bord-btn" ref="buttons" @click="IsClicked(button.id)"></button>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {

  data() {
    return {
      playMode: '2 players',
      twoPlayerMode: true,
      aiMode: false,
      aiIsPlaying: false,
      gameStarted: false,
      gameEnded: false,
      animStarted: false,
      animFinished: false,
      ai: true,
      turnsPlayed: 0,
      finalValue: 0,
      winner: '',
      playedTiles:[],
      players: [
        {
          id: 1,
          name: '',
          isPlaying: true,
        },
        {
          id: 2,
          name: '',
          isPlaying: false,
        }
      ],
      buttons: [
        {
          id: 0,
        },
        {
          id: 1,
        },
        {
          id: 2,
        },
        {
          id: 3,
        },
        {
          id: 4,
        },
        {
          id: 5,
        },
        {
          id: 6,
        },
        {
          id: 7,
        },
        {
          id: 8,
        },
      ]
    }
  },

  methods: {
    PlayModeSelected() {
      switch (this.playMode) {
        case "2 players":
          this.aiMode = false;
          return this.twoPlayerMode = true;
        case "Ai":
          this.twoPlayerMode = false;
          return this.aiMode = true;
      }
    },
    StartGame() {
      this.animStarted = true
      setTimeout(() => {
        this.gameStarted = true;
        setTimeout(() => {
          this.animFinished = true;
          this.isBoxLoaded = true;
        }, 1800);
      }, 1500)
    },
    IsClicked(id) {
      const buttonEl = this.$refs.buttons;
      console.log(this.aiMode)
      if (this.twoPlayerMode) {
        if (this.players[0].isPlaying) {
          if (buttonEl[id].innerHTML != 'X' && buttonEl[id].innerHTML != 'O') {
            buttonEl[id].innerHTML = 'O'
            this.ChangeTurn();
            this.CheckForWinner(buttonEl, this.players[0].name);
          } else {
            alert('cant put that there');
          }
        } else if (this.players[1].isPlaying) {
          if (buttonEl[id].innerHTML != 'X' && buttonEl[id].innerHTML != 'O') {
            buttonEl[id].style.color = 'red';
            buttonEl[id].innerHTML = 'X'
            this.ChangeTurn();
            this.CheckForWinner(buttonEl, this.players[1].name);
          }
          else {
            alert('cant put that there');
          }
        }
      }
      else if (this.aiMode) {
        if (this.players[0].isPlaying) {
          if (buttonEl[id].innerHTML != 'X' && buttonEl[id].innerHTML != 'O') {
            buttonEl[id].innerHTML = 'O'
            this.ChangeTurn();
            this.CheckForWinner(buttonEl, this.players[0].name);
          } else {
            alert('cant put that there');
          }
        }
      }

    },
    ChangeTurn() {
      if (this.twoPlayerMode) {
        if (this.players[0].isPlaying) {
          this.players[0].isPlaying = false;
          this.players[1].isPlaying = true;
          this.turnsPlayed++;
          console.log(this.turnsPlayed);
        } else {
          this.players[1].isPlaying = false;
          this.players[0].isPlaying = true;
          this.turnsPlayed++;
          console.log(this.turnsPlayed);
        }
      }
      else if (this.aiMode) {
        if (this.players[0].isPlaying) {
          this.players[0].isPlaying = false;
          this.aiIsPlaying = true;
          this.turnsPlayed++;
          this.AiTurn();
        }
        else if (this.aiIsPlaying) {
          this.aiIsPlaying = false;
          this.players[0].isPlaying = true;
        }
      }
    },
    AiTurn() {
      this.randomValue();
      const buttonEl = this.$refs.buttons;
      let randomPlayValue = this.finalValue;
      switch (randomPlayValue) {
        case 1:
          const min = 0;
          const max = 9;
          let randomIndex = Math.floor(Math.random() * (max - min) + min);
          if (buttonEl[randomIndex].innerHTML != 'X' && buttonEl[randomIndex].innerHTML != 'O') {
            buttonEl[randomIndex].style.color = 'red';
            buttonEl[randomIndex].innerHTML = 'X'
            this.ChangeTurn();
            break;
          }
          else {
            this.AiTurn();
            break;
          }
        case 2:
          
          buttonEl.forEach(element => 
          {
            if (element.innerHTML === 'O') {
              console.log(this.playedTiles);
            }
          });
          
          break;
      }

    },
    CheckForWinner(row, nameOfPlayer) {
      //Check for winning conditons

      //players one
      //horizontal winning condition
      if (row[0].innerHTML == 'O' && row[1].innerHTML == 'O' && row[2].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[3].innerHTML == 'O' && row[4].innerHTML == 'O' && row[5].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[6].innerHTML == 'O' && row[7].innerHTML == 'O' && row[8].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      //vertical winning condtion
      if (row[0].innerHTML == 'O' && row[3].innerHTML == 'O' && row[6].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[1].innerHTML == 'O' && row[4].innerHTML == 'O' && row[7].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[2].innerHTML == 'O' && row[5].innerHTML == 'O' && row[8].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      //diagonal winning condition
      if (row[0].innerHTML == 'O' && row[4].innerHTML == 'O' && row[8].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[2].innerHTML == 'O' && row[4].innerHTML == 'O' && row[6].innerHTML == 'O') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }

      //PLAYERs TWO win conditions
      //horizontal winning condition
      if (row[0].innerHTML == 'X' && row[1].innerHTML == 'X' && row[2].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[3].innerHTML == 'X' && row[4].innerHTML == 'X' && row[5].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[6].innerHTML == 'X' && row[7].innerHTML == 'X' && row[8].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }

      //vertical winning condtion
      if (row[0].innerHTML == 'X' && row[3].innerHTML == 'X' && row[6].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[1].innerHTML == 'X' && row[4].innerHTML == 'X' && row[7].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[2].innerHTML == 'X' && row[5].innerHTML == 'X' && row[8].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      //diagonal winning condition
      if (row[0].innerHTML == 'X' && row[4].innerHTML == 'X' && row[8].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (row[2].innerHTML == 'X' && row[4].innerHTML == 'X' && row[6].innerHTML == 'X') {
        this.winner = nameOfPlayer;
        this.gameEnded = true;
      }
      if (this.turnsPlayed == 9) {
        console.log('Game ended in draw')
        this.gameEnded = true;
      }
    },
    randomValue() {
      const min = 1;
      const max = 3;

      this.finalValue = Math.floor(Math.random() * (max - min) + min);
    }
  },
}
</script>


<style scoped>
.main-container
{
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid black;
  border-radius: 100px;
  height: 80vh;
}

.selected-mode-container
{
  display: flex;
  width: 100%;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  row-gap: 20px;
  margin-bottom: 80px;
}

.selected-mode-container label
{
  text-decoration: underline;
}

.selected-mode-container select
{
  width: 17%;
  padding: 10px;
  text-align: center;
  font-size: 18px;
  font-weight: 500;
  border: none;
  border-radius: 50px;
  background: rgb(150, 226, 245);
  padding: 20px;
}

.selected-mode-container select:hover
{
  outline: 1px solid black;
  box-shadow: inset 1px 1px 10px 1px black;
  cursor: pointer;
}

.player-name-container
{
  width: 100%;
}

.name-container-section
{
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.player-name-container label
{
  padding: 20px;
}

.player-name-container input
{
  text-align: center;
  padding: 10px;
  width: 100%;
}

.startgame-btn
{
  background: linear-gradient(65deg, rgb(129, 235, 129), rgb(52, 182, 91));
  border: none;
  border-radius: 40px;
  margin-top: 150px;
  width: 400px;
  height: 70px;
  font-size: 20px;
  font-weight: 900;
  transition: all 1.5s ease;
}

.startgame-btn:hover
{
  cursor: pointer;
  background: linear-gradient(120deg, rgb(68, 165, 97), rgb(129, 235, 129), rgb(14, 75, 32), rgb(77, 177, 107));
}

/* GamePlay Bord */
.wrapper
{
  display: flex;
  justify-content: center;
}

.playing-field
{
  max-width: 1010px;
  height: 700px;
}

.row
{
  display: flex;
  flex-wrap: wrap;
  gap: 1px;
}

.box
{
  width: 333px;
  height: 231.6px;
  border: 1px solid black;
}

.bord-btn
{
  width: inherit;
  height: inherit;
  font-size: 100px;
  background: white;
}

.bord-btn:hover
{
  background: greenyellow;
  cursor: pointer;
}

.score-screen
{
  position: absolute;
  min-width: 1400px;
  height: 800px;
  background: rgb(0, 0, 0);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.endgame-name
{
  position: inherit;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;

}

.up-enter-active,
.up-leave-active
{
  transition: all 1.5s ease;
}

.up-enter-from,
.up-leave-to
{
  opacity: 0;
  transform: translateY(-100%)
}
</style>