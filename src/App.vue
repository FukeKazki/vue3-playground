<template>
  <!-- <TodoListComposition /> -->
  <Pomodoro />
  <!-- <CounterProvider>
    <CountUp />
    <RenderCount />
  </CounterProvider> -->
</template>

<script lang="ts">
import TodoList from './components/ToDoList.vue'
import CountUp from './components/CountUp.vue'
import RenderCount from './components/RenderCount.vue'
import CounterProvider from './provider/CounterProvider.vue'
export default {
  name: 'App',
  components: {
    TodoList,
    CounterProvider,
    CountUp,
    RenderCount,
  }
}
</script>

<script setup lang="ts">
import TodoListComposition from './components/composition/TodoList.vue'
import Pomodoro from './components/composition/Pomodoro.vue'
</script>