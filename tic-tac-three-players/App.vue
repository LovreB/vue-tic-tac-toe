<template>
  <h2>{{ nextTurnText }}</h2>
  <div class="board-container">
    <Board :board="boardSquares" @square-clicked="updateBoard" />
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
import Board from "./Board.vue";

const BOARD_SIZE = 16;

const isNext = ref("X");
const boardSquares = ref(Array(BOARD_SIZE).fill(null));

const updateBoard = (i) => {
  if (winner.value || boardSquares.value[i]) return;

  boardSquares.value[i] = isNext.value;
  switchPlayer();
};

const switchPlayer = () => {
  if (isNext.value === "X") {
    isNext.value = "O";
  } else if (isNext.value === "O") {
    isNext.value = "V";
  } else {
    isNext.value = "X";
  }
};

const nextTurnText = computed(() => {
  return winner.value
    ? `The amazing tic-tac-toe winner is: ${winner.value}`
    : `Next up is: ${isNext.value}`;
});

const winner = computed(() => {
  const squares = boardSquares.value;
  const lines = [
    [0, 1, 2],
    [1, 2, 3],
    [4, 5, 6],
    [5, 6, 7],
    [8, 9, 10],
    [9, 10, 11],
    [12, 13, 14],
    [13, 14, 15],
    [0, 4, 8],
    [4, 8, 12],
    [1, 5, 9],
    [5, 9, 13],
    [2, 6, 10],
    [6, 10, 14],
    [3, 7, 11],
    [7, 11, 15],
    [0, 5, 10],
    [5, 10, 15],
    [1, 6, 11],
    [4, 9, 14],
    [2, 5, 8],
    [7, 10, 13],
    [3, 6, 9],
    [6, 9, 12],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
      return squares[a];
  }
  return null;
});
</script>
<style>
h2 {
  text-align: center;
}
.board-container {
  margin: auto;
  max-width: 184px;
  padding-top: 30px;
}
</style>
