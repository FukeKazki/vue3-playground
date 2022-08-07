<template>
  {{ counterContainer.count }}
</template>
<script>
export default {
  inject: ['counterContainer']
}
</script>