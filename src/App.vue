<template>

  <h1>Morpion</h1>

  <p class="player" v-if="playerMark === 'X' ">
    C'est au joueur avec le signe <span>X</span>  qui doit jouer !
  </p>
  <p class="player" v-else>
    C'est au joueur avec le signe <span>O</span> qui doit jouer !
  </p>

  <!-- <section class="container">
    <div @click="addMark" data-index="0" class="case"></div>
    <div @click="addMark" data-index="1" class="case"></div>
    <div @click="addMark" data-index="2" class="case"></div>
    <div @click="addMark" data-index="3" class="case"></div>
    <div @click="addMark" data-index="4" class="case"></div>
    <div @click="addMark" data-index="5" class="case"></div>
    <div @click="addMark" data-index="6" class="case"></div>
    <div @click="addMark" data-index="7" class="case"></div>
    <div @click="addMark" data-index="8" class="case"></div>
  </section> -->

  <section class="container">
    <div v-for="(c, i) in cases" :key="i">
      <div @click="addMark(i)" @keydown="addMark(i)" :data-index="i" class="case">{{ c }}</div>
    </div>
  </section>

  <div v-if="isGame === false">
    <p class="restart" @click="restart" @keydown="restart">Cliquer pour rejouer</p>
  </div>

  <div class="dashboard">
    <div>
      <p>Player 1 (X)</p>
      <p>score: {{ playerOne }}</p>
    </div>
    <div>
      <p>Player 2 (O)</p>
      <p>score: {{ playerTwo }}</p>
    </div>
  </div>

</template>

<script setup lang="ts">
import { ref } from 'vue';

const cases = ref(['', '', '', '', '', '', '', '', '']);
const playerOne = ref<number>(0);
const playerTwo = ref<number>(0);
const answers: string[] = ['', '', '', '', '', '', '', '', ''];
let isGame = true;
const playerMark = ref('X');
let points = true;

const correctAnswers: number[][] = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
];

const addMark = (index: number) => {
  if (!isGame) {
    /* eslint-disable-next-line */
    return}
  /* eslint-disable-next-line */
  else {
    if (playerMark.value === 'X') {
      if (cases.value[index] === '') {
        cases.value.splice(index, 1, 'X');
        answers.splice(index, 1, 'X');
        playerMark.value = 'O';
      }
    }

    if (playerMark.value === 'O') {
      if (cases.value[index] === '') {
        cases.value.splice(index, 1, 'O');
        answers.splice(index, 1, 'O');
        playerMark.value = 'X';
      }
    }

    correctAnswers.forEach((ca) => {
      const [a, b, c]: number[] = ca;

      if (answers[a] && answers[a] === answers[b] && answers[b] === answers[c] && points === true) {
        isGame = false;
        if (answers[a] === 'X' && points === true) {
          playerOne.value++;
          points = false;
        }
        if (answers[a] === 'O' && points === true) {
          playerTwo.value++;
          points = false;
        }
        alert(`Le joueur avec la marque ${answers[a]} a gagné`);
      }
    });
  }
};

const restart = () => {
  for (let i = 0; i < answers.length; i++) {
    answers.splice(i, 1, '');
    cases.value[i] = '';
  }
  points = true;
  isGame = true;
};

</script>

<style lang="scss">
h1 {
  text-align: center;
  color: lightcoral;
}
.player{
  text-align: center;
  font-size: 1.2em;
  span{
    font-size: 1.4em;
    color: blue;
  }
}

.container {
  margin: auto;
  width: 300px;
  height: 300px;
  background-color: lightgray;
  display: flex;
  flex-wrap: wrap;
  border: 10px 10px 15px -1px #000000;

  .case {
    background-color: white;
    width: 95px;
    height: 95px;
    border: 1px solid black;
    display: flex;
    justify-content: center;
    text-align: center;
    align-items: center;
  }
}

.dashboard {
  display: flex;
  justify-content: space-around;
  font-size: 1.5em;
}
.restart{
  text-align: center;
  background-color: aquamarine;
  font-size: 1.2em;
}
</style>
