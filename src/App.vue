<script setup>
import { ref, watch, onMounted } from 'vue'
import Draggable from 'vuedraggable'

const STORAGE_KEY = 'todos-vue'

const todoText = ref('')
const todos = ref([
  { id: 1, completed: false, text: 'プログラミングを勉強する', editing: false },
  { id: 2, completed: true, text: 'スーパーに買い物に行く', editing: false },
])
let number = 3

onMounted(() => {
  const saved = localStorage.getItem(STORAGE_KEY)
  if (saved) {
    todos.value = JSON.parse(saved)
  }
})

watch(todos, (newTodos) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(newTodos))
}, { deep: true})

function addTodo() {
  if(!todoText.value) {
    alert('Todoを入力してください。')
    return
  }
  todos.value.push({
    id: number++,
    completed: false,
    text: todoText.value,
    editing: false
  })
  todoText.value = ''
}

function removeTodo(id) {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

function toggleEdit(todo) {
  todo.editing = !todo.editing
}
</script>

<template>
<h1>Todos</h1>
<div class="todo-form">
  <input type="text" @keyup.enter="addTodo" v-model="todoText" class="add-input" placeholder="Todoを入力してください">
  <button @click="addTodo" class="add-todo">追加する</button>
</div>

<div class="todo-list">
  <h2>Todo一覧</h2>
  <p v-if="todos.length === 0">まだTodoがありません。</p>
  <Draggable v-model="todos" item-key="id" tag="ul" handle=".todo-handle" v-else>
    <template #item="{ element: todo }">
      <li>
        <span class="todo-desc">
          <span class="todo-icon">
            <span class="todo-handle">
              <span class="todo-bar"></span>
            </span>
            <input type="checkbox" v-model="todo.completed" class="todo-checkbox">
          </span>
          <span v-if="!todo.editing" :class="{ 'todo-completed': todo.completed }">{{ todo.text }}</span>
          <input v-else type="text" v-model=todo.text @keyup.enter="todo.editing = false" class="edit-input">
        </span>
        <span class="todo-actions">
          <button @click="toggleEdit(todo)" class="todo-button -editing">{{ todo.editing ? '保存' : '編集' }}</button>
          <button @click="removeTodo(todo.id)" class="todo-button -remove">削除</button>
        </span>
      </li>
    </template>
  </Draggable>
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
  padding: 0.8rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
  position: relative;
  padding-left: 6.4rem;
}

@media (max-width:767px) {
  li {
    flex-direction: column;
    align-items: flex-end;
    gap: 0.6rem;
    padding-left: 7.2rem;
  }
}

.todo-handle {
  cursor: grab;
  width: 1.4rem;
  aspect-ratio: 1;
  position: relative;
}

@media (max-width:767px) {
  .todo-handle {
    width: 2rem;
  }
}

.todo-bar {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  left: 0;
  width: 100%;
  height: 1px;
  background-color: #333;
}

.todo-bar::before, .todo-bar::after {
  content: '';
  width: 100%;
  height: 1px;
  background-color: #333;
  position: absolute;
  left: 0;
  top: -4px;
}

@media (max-width:767px) {
  .todo-bar::before, .todo-bar::after {
    top: -5px;
  }
}

.todo-bar::after {
  top: 4px;
}

@media (max-width:767px) {
  .todo-bar::after {
    top: 5px;
  }
}

.todo-checkbox {
  width: 1.5rem;
  aspect-ratio: 1;
}

@media (max-width:767px) {
  .todo-checkbox {
    width: 1.8rem;
    aspect-ratio: 1;
  }
}

.todo-handle:active {
  cursor: grabbing;
}

.todo-icon {
  display: flex;
  align-items: center;
  gap: 1rem;
  position: absolute;
  left: 0.8rem;
  top: 50%;
  transform: translateY(-50%);
}

.todo-desc{
  display: flex;
  align-items: center;
  gap: 2.4rem;
  width: 100%;
}

.edit-input {
  font-size: 1.6rem;
  line-height: 1.75;
  letter-spacing: 0.04em;
  padding: 0;
  width: 100%;
}

.todo-actions {
  display: flex;
  gap: 8px;
}

.todo-button {
  background-color: #37e91c;
  border: none;
  border-radius: 4px;
  color: #fff;
  padding: 2px 8px;
  background-color: #e74c3c;
  transition: .2s;
  width: max-content;
}

@media (max-width:767px) {
  .todo-button {
    font-size: 1.6rem;
  }
}

.todo-button:hover {
  background-color: #c0392b;
}

.todo-button.-editing {
  background-color: #3498db;
}

.todo-button.-editing:hover {
  background-color: #2980b9;
}
</style>