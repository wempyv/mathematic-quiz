<template>
  <div class="container">
    <section class="section-game-start" v-if="!isPlaying && !gameOver">
      <h2>Mathematic<br />Quiz</h2>
      <button class="btn btn-warning btn-lg mt-3" @click="playingGame">
        Play Now <i class="fa fa-play"></i>
      </button>
    </section>
    <section class="section-game-play" v-if="isPlaying">
      <div class="answer-group">
        <div class="timers">
          <span class="timer">{{ timeleft }}</span>
        </div>
        <div class="question mt-3">
          <h1 class="question">
            {{ variabel1 }} {{ operatorChoose === "*" ? "x" : operatorChoose }}
            {{ variabel2 }} =
          </h1>
        </div>
      </div>
      <form class="mt-3" @submit.prevent="userSubmitAnswer">
        <input
          type="number"
          placeholder="Input your answer here"
          class="text-center user-answer"
          v-model="answer"
          autofocus
        />
        <br />
        <button v-if="answer" type="submit" class="btn btn-warning btn-sm mt-2">
          Submit
        </button>
      </form>
    </section>
    <section class="section-game-fail" v-if="gameOver">
      <h1>Failed !! <br />Try Again</h1>
      <button @click="tryAgain" class="btn btn-warning btn-lg try-again mt-2">
        <i class="fa fa-repeat"></i>
      </button>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      answer: "",
      isPlaying: false,
      variabel1: null,
      variabel2: null,
      gameOver: false,
      operator: ["+", "-", "*"],
      operatorChoose: null,
      result: "",
      timeleft: 30,
    };
  },
  methods: {
    playingGame() {
      this.timeleft = 30;
      this.isPlaying = true;
      this.questionRandom();
    },
    questionRandom() {
      let operator = ["+", "-", "*"];
      let operatorRandom = Math.floor(Math.random() * 3);

      this.variabel1 = parseInt(Math.random() * 20);
      this.variabel2 = parseInt(Math.random() * 20);

      this.operatorChoose = operator[operatorRandom];
      this.sumVariabel();
    },
    userSubmitAnswer() {
      this.answer = parseInt(this.answer);
      if (this.answer === this.result) {
        this.playingGame();
        this.answer = "";
      } else {
        this.isPlaying = false;
        this.gameOver = true;
      }
    },
    tryAgain() {
      this.playingGame();
      this.answer = "";
      this.gameOver = false;
    },
    sumVariabel() {
      if (this.operatorChoose === "+") {
        this.result = this.variabel1 + this.variabel2;
      } else if (this.operatorChoose === "-") {
        this.result = this.variabel1 - this.variabel2;
      } else {
        this.result = this.variabel1 * this.variabel2;
      }
    },
  },
  mounted() {
    let downloadTimer = setInterval(() => {
      if (this.timeleft <= 0) {
        this.isPlaying = false;
        this.gameOver = true;
      }
      this.timeleft -= 1;
    }, 1000);
  },
};
</script>

<style>
.container {
  color: rgb(0, 255, 34);
}
.answer-group {
  background-color: rgba(2, 0, 97, 0.4);
  padding: 35px;
  border-radius: 9px;
}
input {
  background: transparent;
  color: white;
  border-radius: 8px;
  border: none;
  font-size: 20px;
}
input:focus {
  outline: none;
}
.question {
  font-size: 50px;
}
.timer {
  padding: 10px 12px;
  background-color: rgb(180, 0, 0);
  border-radius: 50%;
  color: white;
  font-size: 25px;
}

.try-again {
  border-radius: 50%;
}
</style>
