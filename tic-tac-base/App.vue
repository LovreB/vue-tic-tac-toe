<template>
  <h2>HELLO CODEPUBERS!</h2>
  <div class="board-container">
    <Board />
  </div>
</template>
<script setup>
import Board from "./Board.vue";

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
