<template>
  <Square
    v-for="(value, i) in board"
    :key="i"
    :value="board[i]"
    @click="emits('squareClicked', i)"
  />
</template>
<script setup>
import { ref } from "vue";
import Square from "./Square.vue";

const props = defineProps({
  board: {
    type: Array,
  },
});
const emits = defineEmits(["squareClicked"]);
</script>

<style>
.row {
  display: block;
  width: 100%;
}
</style>
