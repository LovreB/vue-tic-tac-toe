<template>
  <Square
    v-for="(value, index) in boardSquares"
    :key="index"
    :value="value"
    @click="emits('squareClicked', index)"
  />
</template>
<script setup>
import Square from "./Square.vue";
import { ref, computed, toRefs } from "vue";
const props = defineProps({
  boardSquares: {
    type: Array,
  },
});
const { boardSquares } = toRefs(props);
const emits = defineEmits(["squareClicked"]);
</script>

<style>
.row {
  display: block;
  width: 100%;
}
</style>
