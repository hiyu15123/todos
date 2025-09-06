<script setup>
import { ref } from 'vue'

const todoText = ref('')
const todos = ref([])
let number = 1

function addTodo() {
  if(!todoText.value) {
    alert('入力してください。')
  }
  todos.value.push({
    id: number++,
    completed: false,
    text: todoText.value,
  })
  todoText.value = ''
}

function removeTodos() {
  todos.value = todos.value.filter(todo => !todo.completed)
}
</script>

<template>
<h1>ToDoアプリ</h1>
<input type="text" v-model="todoText">
<button @click="addTodo">追加する</button>
<button @click="removeTodos">完了済みのものを削除</button>

<p v-if="todos.length === 0">まだTodoがありません。</p>
<ul v-else>
  <li v-for="todo in todos" :key="todo.id">
    <input type="checkbox" v-model="todo.completed">
    <span :class="{ 'todo-completed': todo.completed }">{{ todo.text }}</span>
  </li>
</ul>

</template>

<style scoped>
.todo-completed {
  text-decoration: line-through;
}
</style>