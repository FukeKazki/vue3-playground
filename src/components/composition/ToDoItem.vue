<template>
  <li class="todo-item" v-bind:class="{ 'done': done }" v-on:click="handleClick">
    <span v-if="done">✅</span>
    <slot />
  </li>
</template>
<script setup lang="ts">
import { ref, reactive, computed, defineProps } from 'vue'
const props = defineProps({
  done: Boolean
})

const emit = defineEmits(['toggle'])
const handleClick = () => {

}
</script>