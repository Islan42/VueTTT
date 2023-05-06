<script setup>
import { ref, computed } from 'vue'

import Board from './components/Board.vue'

const historyGame = ref([Array(9).fill(null)])
const currentMove = ref(0)
const xIsNext = computed(() => currentMove.value % 2 === 0)
const currentSquares = computed(() => historyGame.value[currentMove.value])

function handlePlay(nextSquares) {
  const nextHistory = [...historyGame.value.slice(0, currentMove.value + 1), nextSquares]
  historyGame.value = nextHistory
  currentMove.value = nextHistory.length - 1
}

function jumpTo(nextMove) {
  currentMove.value = nextMove
}

const moves = computed(() => {
  return historyGame.value.map((squares, move) => {
    let description;
    if (move > 0) {
      description = `Go to move #${move}`
    } else {
      description = 'Go to game start'
    }
    return {description: description}
  })
})
</script>

<template>
  <div className="game">
    <div className="game-board">
      <Board :xIsNext="xIsNext" :squares="currentSquares" @click-play="handlePlay" />
    </div>
    <div className="game-info">
      <ol>
        <li v-for="(move, index) in moves" :key="index">
          <button @click="() => jumpTo(index)">{{move.description}}</button>
        </li>
      </ol>
    </div>
  </div>
</template>