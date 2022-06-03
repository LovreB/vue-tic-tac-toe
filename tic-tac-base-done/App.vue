<template>
  <h2>{{ nextTurnText }}</h2>
  <div class="board-container">
    <Board :board="board" @square-clicked="updateBoard" />
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
import Board from "./Board.vue";

const isNext = ref("X");
const board = ref(Array(9).fill(null));

const updateBoard = (i) => {
  const winner = calculateWinner(board.value);
  if (winner || board.value[i]) return;

  board.value[i] = isNext.value;
  isNext.value = isNext.value === "X" ? "O" : "X";
};

const nextTurnText = computed(() => {
  const winner = calculateWinner(board);
  return winner
    ? `The amazing tic-tac-toe winner is: ${winner}`
    : `Next up is: ${isNext.value}`;
});

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
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
      return squares[a];
  }
  return null;
};
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
