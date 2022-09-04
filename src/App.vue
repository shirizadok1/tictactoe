<script setup>
import { ref, computed } from 'vue'

const player = ref('X');
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],

])

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]; //a,b,c will be the indexs of the matrix to check winners, it can be 0,1,2 or 3,4,5 and so on(pulling the numbers and we put them as a,b,c)
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a]; //if they win we return whoever is in that cell because thats the winner
    }
  }
  return null;
}


const winner = computed(() => calculateWinner(board.value.flat)) //because we are using compted we need to declate board.value and not just board and.flat because we want to flat it to an array

const MakeMove = (x,y) => {
  if(winner.value) return   // if we have a winner then the game is over

  if(board.value[x][y] !== '') return   //means that the cell is not empty and someone already made a move there so we return

  board.value[x][y] = player.value  //our board in those cells is equal to whoever made that move
  
  player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]
  player.value = 'X'  //we reset the game and the first player will be X again
}
</script>

<template>
  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">

        <div v-for="(cell, y) in row" :key="y" @click="MakeMove(x,y)" :class="`border border-black w-20 h-20 hover:bg-teal-700 flex items-center
        justify-center material-icons-outlined text-4xl cursor-pointer`">
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : ''}}


        </div>

      </div>
    </div>
    <h2 v-if="winner" class="text-6xl font-bold mb-8">Player '{{winner}}' wins!</h2>
     <!-- v-if means if we have a winner only then we will declare him -->
    <button @click="ResetGame" class="px-4 py-2 bg-blue-500 rounded uppercase font-bold hover:bg-blue-600 duration-300">Reset Game</button>
  </main>
</template>

<style>
</style>
