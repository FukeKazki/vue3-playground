<template>
  <li class="todo-item" v-bind:class="{ 'done': done }" v-on:click="handleClick">
    <span v-if="done">✅</span>
    <slot/>
  </li>
</template>
<script lang="ts">
export default {
  props: {
    done: {
      type: Boolean
    }
  },
  emits: ['toggle'],
  methods: {
    handleClick() {
      this.$emit('toggle')
    }
  }
}
</script>