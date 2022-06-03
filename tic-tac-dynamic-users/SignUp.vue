<template>
  <div>
    <p>Player 1 name:</p>
    <input v-model="playerOneName" />
  </div>
  <div>
    <p>Player 2 name:</p>
    <input v-model="playerTwoName" />
  </div>
  <button @click="emitPlayerNames">Start Game</button>
</template>
<script setup>
import { ref } from "vue";

const emits = defineEmits(["player-names-updated"]);
const playerOneName = ref("");
const playerTwoName = ref("");

const emitPlayerNames = () => {
  if (playerOneName && playerTwoName)
    emits("player-names-updated", [playerOneName.value, playerTwoName.value]);
};
</script>
<style>
button {
  margin-top: 20px;
}
</style>
