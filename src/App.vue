<script setup>
import { computed, ref } from "vue"

const player = ref("X")
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
])
const calculateWinner = (board) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ]

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a]
    }
  }

  return null
}
const winner = computed(() => calculateWinner(board.value.flat()))
const makeMove = (x, y) => {
  if (winner.value) return
  if (board.value[x][y]) return
  board.value[x][y] = player.value
  player.value = player.value === "X" ? "O" : "X"
}

const resetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ]
  player.value = "X"
}
</script>
<template>
  <main class="pt-8 bg-gray-700 min-h-screen text-center text-white">
    <h1 class="text-3xl mb-6">Tic Tac Toe</h1>
    <h2 class="text-2xl mb-8">Player {{ player }} 's turn</h2>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          :class="`border border-white w-24 h-24 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
        >
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
      <div class="text-center">
        <h2 v-if="winner" class="text-3xl font-bold mb-8 mt-5">
          Player '{{ winner }}' wins!
        </h2>
        <button
          @click="resetGame"
          class="p-3 mt-4 bg-blue-500 hover:bg-blue-700 rounded-md duration-200"
        >
          Reset
        </button>
      </div>
    </div>
  </main>
</template>