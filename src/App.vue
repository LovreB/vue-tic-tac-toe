<template>
  <h2>HELLO CODEPUBERS!</h2>
  <div class="board-container">
    <Board />
  </div>
</template>
<script setup>
import Board from "./Board.vue"; // Imports the Board component so that it can be used in the template
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
