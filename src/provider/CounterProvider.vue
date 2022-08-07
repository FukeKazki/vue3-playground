<template>
  <slot />
</template>
<script lang="ts">
export default {
  data() {
    return {
      container: {
        count: 0
      }
    }
  },
  provide() {
    const counterContainer = this.container
    return { counterContainer }
  }
}
</script>