<template>

  <h1>Morpion</h1>

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
      <div @click="addMark" @keydown="restart" :data-index="i" class="case">{{ c }}</div>
    </div>
  </section>

  <!-- <div v-if="isGame === false">
    <p @click="restart">Cliquer pour rejouer</p>
  </div> -->
  <div>
    <p @click="restart" @keydown="restart">Cliquer pour rejouer</p>
  </div>

  <div class="dashboard">
    <div>
      <p>Player 1 (X): {{ playerOne }}</p>
      <p>score: </p>
    </div>
    <div>
      <p>Player 2 (O): {{ playerTwo }}</p>
      <p>score: </p>
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
let playerMark = 'X';

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

const addMark = (e: any) => {
  if (!isGame) {
    /* eslint-disable-next-line */
    return}
  /* eslint-disable-next-line */
  else {
    /* eslint-disable-next-line */
    const index: number = e.target.dataset.index;

    if (playerMark === 'X') {
      if (!e.target.textContent) {
        e.target.textContent = 'X';
        e.target.style.fontSize = '3em';
        answers.splice(index, 1, 'X');
        playerMark = 'O';
      }
    }

    if (playerMark === 'O') {
      if (!e.target.textContent) {
        e.target.textContent = 'O';
        e.target.style.fontSize = '3em';
        answers.splice(index, 1, 'O');
        playerMark = 'X';
      }
    }

    correctAnswers.forEach((ca) => {
      const [a, b, c]: number[] = ca;

      if (answers[a] && answers[a] === answers[b] && answers[b] === answers[c]) {
        isGame = false;
        if (answers[a] === 'X') {
          playerOne.value++;
        }
        if (answers[a] === 'O') {
          playerTwo.value++;
        }
        alert(`Le joueur avec la marque ${answers[a]} a gagné`);
      }
    });
  }
};

const restart = () => {
  console.log(cases.value);
  for (let i = 0; i < answers.length; i++) {
    answers.splice(i, 1, '');
    cases.value[i] = '';
  }
  console.log(cases.value);
  isGame = true;
};

</script>

<style lang="scss">
h1 {
  text-align: center;
  color: lightcoral;
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
</style>
