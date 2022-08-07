<template>
  <input type="text" v-model="inputValue">
  <button v-on:click="handleClick">
    ToDoを追加
  </button>
  <input type="text" v-model="filterValue" placeholder="フィルタテキスト">
  <ul>
    <ToDoItem v-for="todo in filteredTodoItems" v-bind:key="todo.id" :text="todo.text" :done="todo.done"
      v-on:toggle="todo.done = !todo.done">
      {{ todo.text }}
    </ToDoItem>
  </ul>
</template>
<script setup lang="ts">
import { ref, reactive, computed } from 'vue'
const todoItems = reactive([
  { id: 1, done: false, text: 'Go out to sea' },
  { id: 2, done: false, text: 'Invite the first member' }
])

const inputValue = ref('')
const handleClick = () => {
  const id = todoItems.length + 1;
  todoItems.push(
    { id, done: false, text: inputValue.value }
  )
  inputValue.value = ''
}

const filterValue = ref('')
const filteredTodoItems = computed(() => {
  if (!filterValue.value) {
    return todoItems
  }
  return todoItems.filter(
    todo => todo.text.includes(filterValue.value)
  )
})
</script>