<script setup>

import {ref,computed} from 'vue'

const player = ref('x')
const board = ref([
  ['','',''],
  ['','',''],
  ['','','']
])

const calculateWinner = (squares) =>{
  console.log(squares);
  const lines = [
    [0,1,2],
    [3,4,5],
    [6,7,8],
    [0,3,6],
    [1,4,7],
    [2,5,8],
    [0,4,8],
    [2,4,6]
  ];

  for (let i=0;i<lines.length;i++){
    const [a,b,c] = lines[i]
    if(squares[a] && squares[a] === squares[b] && squares[a] === squares[c]){
      return squares[a]
    }
  }
  return null
}

const winner = computed(() => calculateWinner(board.value.flat()))

const makeMove = (x,y) => {
  if(winner.value) return
  
  if(board.value[x][y] !== '') return

  board.value[x][y] = player.value
  player.value = player.value === 'x' ? 'O' : 'x'
}

const resetGame = ()=>{
  board.value = [
    ['','',''],
    ['','',''],
    ['','','']
  ]
  player.value = 'x'
}


</script>

<template>
  <main class="pt-8 text-center">
    <h1 class="mb-8 text-3xl font-bold uppercase" >Tic Tac Toe</h1>
    <h3 class="text-xl mb-4">Player {{player}}`s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for = "(row,x) in board" 
           :key = "x" 
          class="flex">        
        <div 
          v-for="(cell,y) in row" 
          :key = "y" 
          @click="makeMove(x,y)" 
          :class="`border border-white w-20 h-20 hover:bg-gray-700 flex items-center
          justify-center material-icons-outlined text-4xl cursor-pointer ${cell === 'x' ? 'text-pink-500' : 'text-blue-400'}`" >
          {{cell === 'x' ? 'close' : cell === 'O' ? 'circle':''}}

        </div>
      </div>
      <div class="text-center mt-8">
        <h2 v-if="winner" class="text-6xl font-bold mb-8"> Play '{{ winner }}' wins</h2>
        <button @click="resetGame" class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-1000">reset game</button>
      </div>

    </div>
  </main>
  
</template>

<style>
body {
	@apply bg-gray-800 text-white;
}
</style>
