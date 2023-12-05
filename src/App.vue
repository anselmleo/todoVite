<script setup>
import { ref, computed, watch, onMounted } from 'vue';
  const username = "Henry"
  const todoDescriptionInput = ref('')
  const todoCategoryInput = ref(null)
  const todos = ref([])
  const todosAsc = computed(() => todos.value.sort((a, b) => b.date - a.date))
  const addTodo = () => {
    if(todoDescriptionInput.value === '' || todoCategoryInput.value === null) return alert('Please enter a todo')
    let todoList = JSON.parse(localStorage.getItem('todos')) || []
    todos.value.push({
      id: todoList.length+1, 
      todoDescription: todoDescriptionInput.value, 
      todoCategory: todoCategoryInput.value,
      todoStatus: false, 
      date: new Date().getTime()})
    todoDescriptionInput.value = ''
    todoCategoryInput.value = null
  }

  onMounted(() => {
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })

  watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  }, {deep: true})


</script>

<template>
  <main class="app">
    <section class="greeting">
      <h3 class="title">What's up, {{username}}</h3>
    </section>
    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list today?</h4>
        <input type="text" v-model="todoDescriptionInput" class="todoDescription">
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input type="radio" v-model="todoCategoryInput" name="todoCategory" value="Personal">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
          <label>
            <input type="radio" v-model="todoCategoryInput" name="todoCategory" value="Business">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <input type="submit" value="Add todo">

        </div>
      </form>
    </section>
    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">
        <div v-for="todo in todosAsc" :key="todo.id" :class="`todo-item ${todo.todoStatus && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.todoStatus">
            <span :class="`bubble ${todo.todoCategory}`">
            </span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.todoDescription">
          </div>
        </div>
      </div>
    </section>
  </main>
 </template>

