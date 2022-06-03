<template>
  <h2>{{ nextTurnText }}</h2>
  <div class="board-container">
    <Board :board="boardSquares" @square-clicked="updateBoard" />
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
import Board from "./Board.vue";

const isNext = ref(0);
const boardSquares = ref(Array(9).fill(null));
const nextPlayer = computed(() => props.players[isNext.value]);

const props = defineProps({
  players: Array,
});

const updateBoard = (i) => {
  if (winner.value || boardSquares.value[i]) return;

  const numberOfPlayers = props.players.length;
  boardSquares.value[i] = nextPlayer.value.token;
  isNext.value = (isNext.value + 1) % numberOfPlayers;
};

const nextTurnText = computed(() => {
  const winningPlayer = winner.value;
  return winningPlayer
    ? `The amazing tic-tac-toe winner is: ${winningPlayer.name} (${winningPlayer.token})`
    : `Next up is: ${nextPlayer.value.name} (${nextPlayer.value.token})`;
});

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
      return props.players.find((player) => player.token === squares[a]);
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
  max-width: 138px;
  padding-top: 30px;
}
</style>
