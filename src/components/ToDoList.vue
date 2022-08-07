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
<script lang="ts">
import ToDoItem from "./ToDoItem.vue";
export default {
  data() {
    return {
      inputValue: "",
      todoItems: [
        { id: 1, text: "Go out to sea", done: false },
        { id: 2, text: "Invite the first member", done: false }
      ],
      filterValue: "",
    };
  },
  computed: {
    filteredTodoItems() {
      if (!this.filterValue) {
        return this.todoItems;
      }
      return this.todoItems.filter((todo) => {
        return todo.text.includes(this.filterValue);
      });
    }
  },
  methods: {
    handleClick() {
      this.todoItems.push({
        id: this.todoItems.length + 1,
        text: this.inputValue
      });
      this.inputValue = "";
    }
  },
  components: { ToDoItem }
}
</script>
<style>
.todo-item.done {
  background-color: #3fb983;
  color: #fff;
}
</style>