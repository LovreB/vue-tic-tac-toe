<template>
  <Game v-if="players" :players="players" />
  <SignUp v-else @player-names-updated="updatePlayers" />
</template>
<script setup>
import { ref, computed } from "vue";
import Game from "./Game.vue";
import SignUp from "./SignUp.vue";

const players = ref();

const updatePlayers = (playerNames) => {
  const newPlayers = [
    { name: playerNames[0], token: "X" },
    { name: playerNames[1], token: "O" },
  ];
  players.value = newPlayers;
};
</script>
<style></style>
