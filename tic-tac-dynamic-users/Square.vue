<template>
  <span class="square">{{ value }}</span>
</template>
<script setup>
defineProps({
  value: String,
});
</script>
<style>
.square {
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 44px;
  height: 44px;
  text-align: center;
  min-width: 44px;
  cursor: pointer;
}
</style>
