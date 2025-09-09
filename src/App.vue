<script setup>
import { ref } from 'vue'

const todoText = ref('')
const todos = ref([
  { id: 1, completed: false, text: 'プログラミングを勉強する' },
  { id: 2, completed: true, text: 'スーパーに買い物に行く' }
])
let number = 1

function addTodo() {
  if(!todoText.value) {
    alert('Todoを入力してください。')
    return
  }
  todos.value.push({
    id: number++,
    completed: false,
    text: todoText.value,
  })
  todoText.value = ''
}

function removeTodo(id) {
  todos.value = todos.value.filter(todo => todo.id !== id)
}
</script>

<template>
<h1>Todoアプリ</h1>
<div class="todo-form">
  <input type="text" v-model="todoText" class="add-input" placeholder="Todoを入力してください">
  <button @click="addTodo" class="add-todo">追加する</button>
</div>

<div class="todo-list">
  <h2>Todo一覧</h2>
  <p v-if="todos.length === 0">まだTodoがありません。</p>
  <ul v-else>
    <li v-for="todo in todos" :key="todo.id">
      <span class="todo-desc">
        <input type="checkbox" v-model="todo.completed">
        <span :class="{ 'todo-completed': todo.completed }">{{ todo.text }}</span>
      </span>
      <button @click="removeTodo(todo.id)" class="todo-button">削除</button>
    </li>
  </ul>
</div>


</template>

<style scoped>
button,input[type="checkbox"] {
  cursor: pointer;
}
.todo-form {
  display: flex;
  align-items: center;
}
.todo-form + .todo-list {
  margin-top: 60px;
}
.add-input {
  padding: 8px 16px;
  width: 100%;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
  line-height: 1.75;
}
.add-todo {
  background-color: #f2f2f2;
  border: 1px solid #ddd;
  border-radius: 0 4px 4px 0;
  border-left: none;
  width: 100px;
  min-width: 100px;
  padding: 8px 16px;
  position: relative;
  line-height: 1.75;
}

.todo-completed {
  text-decoration: line-through;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  border-bottom: 1px solid #ddd;
  padding-block: 8px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 40px;
}
.todo-desc {
  display: flex;
  gap: 24px;
}
.todo-button {
  background-color: #37e91c;
  border: none;
  border-radius: 4px;
  color: #fff;
  padding: 2px 8px;
  background-color: #e74c3c;
  transition: .2s;
}
.todo-button:hover {
  background-color: #c0392b;
}

</style>