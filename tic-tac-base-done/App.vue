<template>
  <h2>{{ nextTurnText }}</h2>
  <div class="board-container">
    <Board :boardSquares="boardSquares" @square-clicked="updateBoard" />
  </div>
</template>
<script setup>
import Board from "./Board.vue";
import { ref, computed } from "vue";

const boardSquares = ref(Array(9).fill(null));

const updateBoard = (index) => {
  if (winner.value || boardSquares.value[index]) return;

  boardSquares.value[index] = isNext.value;
  isNext.value = isNext.value === "X" ? "O" : "X";
};
const isNext = ref("X");

const winner = computed(() => {
  const squares = boardSquares.value;
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
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
      return squares[a];
  }
  return null;
});

const nextTurnText = computed(() => {
  return winner.value
    ? `The amazing tic-tac-toe winner is: ${winner.value}`
    : `Next up is: ${isNext.value}`;
});
</script>
<style>
h2 {
  text-align: center;
}
.board-container {
  margin: auto;
  max-width: 138px;
  padding-top: 30px;
}
</style>
