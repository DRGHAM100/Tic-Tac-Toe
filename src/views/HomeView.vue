<template>
  <main class="pt-8 text-center dark:bg-gray-800 dark:text-white min-h-screen">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <h1 class="text-xl">Player {{ player }}'s turn</h1>

    <div class="flex flex-col items-center mt-8 mb-8">
      <div class="flex" v-for="(row,x) in board" :key="x">
        <div 
          v-for="(cell,y) in row" 
          :key="y" @click="makeMove(x,y)" 
          class="border border-white w-20 h-20 hover:bg-gray-700 cursor-pointer flex items-center justify-center text-4xl text-pink-600 material-icons-outlined"
          :class="{'text-pink-500' : cell === 'X','text-blue-500' : cell === 'O'}"
          >
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
        </div>
      </div>
    </div>

    <h1 class="text-6xl font-bold mb-8 " v-if="winner">Player {{ (winner) }} wins</h1>

    <h1 class="text-6xl font-bold mb-8 " v-if="movesCount == 9 && !winner">Draw</h1>

    <button @click="resetGame" class="bg-white text-gray-700 font-bold text-2xl py-2 px-6 rounded-lg" >Reset Game</button>
  </main>
</template>

<script setup>
import {ref,computed} from 'vue';

const player = ref('X');

const movesCount = ref(0);

const board = ref([
  ['','',''],
  ['','',''],
  ['','','']
]);

const calculateWinner = (squares) => {
  const Lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
  ];
  for (let i = 0; i < Lines.length; i++) {
      const [a, b, c] = Lines[i];
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
        return squares[a];
      }
    }
  return null;
};


const winner = computed(() => calculateWinner(board.value.flat()));

const makeMove = (x,y) => {
  if (winner.value) return;

  if (board.value[x][y] !== '') return;

  board.value[x][y] = player.value;

  player.value = player.value === 'X' ? 'O' : 'X';

  movesCount.value += 1;
}

const resetGame = () => {
  board.value = [
    ['','',''],
    ['','',''],
    ['','','']
  ];

  player.value = 'X';

  movesCount.value = 0;
}
</script>
