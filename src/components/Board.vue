<script setup>
import { computed } from 'vue'
import Square from './Square.vue'

const props = defineProps(['xIsNext', 'squares'])
const emit = defineEmits(['click-play'])

const winner = computed(() => calculateWinner(props.squares))

function handleClick(i) {
  if (props.squares[i] || calculateWinner(props.squares)) {
    return
  }
  const nextSquares = props.squares.slice() //Tentar usar Espalhamento
  nextSquares[i] = props.xIsNext ? 'X' : 'O'
  emit('click-play', nextSquares)
}
function calculateWinner(squares) {
  const lines = [
    [0,1,2], [3,4,5], [6,7,8],
    [0,3,6], [1,4,7], [2,5,8],
    [0,4,8], [2,4,6]
  ]
  for (let i = 0; i < lines.length; i++) {
    const [a,b,c] = lines[i]
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a]
    }
  }
  return null
}
</script>

<template>
  <div>
    <div className="status">{{winner ? `Winner: ${winner}` : `Next play: ${props.xIsNext ? 'X' : 'O'}`}}</div>
    <div className="board-row">
      <Square :value="squares[0]" @square-click="() => handleClick(0)" />
      <Square :value="squares[1]" @square-click="() => handleClick(1)" />
      <Square :value="squares[2]" @square-click="() => handleClick(2)" />
    </div>
    <div className="board-row">
      <Square :value="squares[3]" @square-click="() => handleClick(3)" />
      <Square :value="squares[4]" @square-click="() => handleClick(4)" />
      <Square :value="squares[5]" @square-click="() => handleClick(5)" />
    </div>
    <div className="board-row">
      <Square :value="squares[6]" @square-click="() => handleClick(6)" />
      <Square :value="squares[7]" @square-click="() => handleClick(7)" />
      <Square :value="squares[8]" @square-click="() => handleClick(8)" />
    </div> 
  </div>
</template>