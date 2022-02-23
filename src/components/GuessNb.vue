<template>
  <div class="container">
    <h1 class="title">Guess a number between 1 and 100</h1>
    <div v-if="newGame">
      <form class="form" @submit.prevent="checkAnswer">
        <label class="nbLabel" for="number">Enter a number</label>
        <input class="nbInput" type="number" v-model="playerAnswer">
        <button class="btn" type="submit">Enter</button>
      </form>
      <p class="message" v-bind:class="{ green: success }" v-if="message !== ''">{{ message }}</p>
      <button class="btn" v-if="success === true" @click="startNewGame">Start a new game !</button>
    </div>

    <div v-else>
      <button class="btn" @click="renderRandomNb">Start the game !</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GuessNb',
  data(){
    return {
      newGame: false,
      nbToGuess: null,
      playerAnswer: '',
      message: '',
      success: false,
    };
  },
  methods: {
    startNewGame() {
      this.newGame = false; // reset newGame to false to show only the "start the game !" button (v-else)
      this.nbToGuess = null; // reset nbToGuess to null
      this.playerAnswer = ''; // reset the player's answer
      this.message = ''; // empty the message displayed, so nothing show
      this.success = false; // reset the success to false (for our message css property)
    },
    renderRandomNb() {
      this.newGame = true; // we start a brand new game
      this.nbToGuess = Math.floor(Math.random() * 100); // will generate a random number (integer) between 0 and 100
      console.log(this.nbToGuess);
    },
    checkAnswer() {
      // check if the player has enter an answer
      if (this.playerAnswer == '') {
        this.message = 'A number must be entered !';
      
      // check if the number entered is negative
      } else if (this.playerAnswer <= 0) {
        this.message = 'Number can not be negative or equal to 0 !';

      // check if the player has enter inferior to the number to guess
      } else if (this.playerAnswer < this.nbToGuess) {
        this.message = 'Too low !';

      // check if the player has enter superior to the number to guess
      } else if (this.playerAnswer > this.nbToGuess) {
        this.message = 'Too high !';
      
      // check if the player's answer is equal to the number to guess
      } else if (this.playerAnswer === this.nbToGuess) {
        this.message = 'Youhou ! You guessed the number ! Congratulations !';
        this.success = true; // set success to true -> will allow to bind our css class for our message
      }
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  width: 50%;
  margin: 0 auto;
  padding-bottom: 1rem;
}

.title {
  color: rgb(56, 73, 119);
  width: 100%;
}

.nbLabel, .nbInput {
  margin-right: 0.5rem;
}

.nbInput {
  padding: 0.3rem 0;
}

.btn {
  background-color: rgb(56, 73, 119);
  border: solid 1px rgb(56, 73, 119);
  padding: 0.3rem 0.5rem;
  color: #fff;
  cursor: pointer;
}

.message {
  color: rgb(201, 39, 39);
}

.green {
  color: rgb(57, 139, 24);
}
</style>
