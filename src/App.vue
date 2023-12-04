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
    todos.value.push({id: todoList.length+1, todoDescription: todoDescriptionInput.value, todoCategory: todoCategoryInput.value, date: new Date().getTime()})
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
  <section class="container">
    <section class="header">
      <h3>What's up, {{username}} {{todoDescriptionInput}} {{todoCategoryInput}}</h3>
    </section>
    <section class="createTodo">
      <form action="" @submit.prevent="" class="todoForm">
        <section class="inputTodo">
          <h2>CREATE A TODO</h2>
          <p>What's on your todo list today?</p>
          <input type="text" v-model="todoDescriptionInput" class="todoDescription">
        </section>
        <section class="selectCategory">
          <p>Pick a category</p>
          <label for="todoCategory">
            <input type="radio" v-model="todoCategoryInput" name="todoCategory" value="Personal">
            Personal
            <input type="radio" v-model="todoCategoryInput" name="todoCategory" value="Business">
            Business
          </label>
        </section>
        <section class="addTodo">
          <br>
          <input type="submit" @click="addTodo" class="addTodo">
        </section>
        <section class="showTodolist">
          <h3>TODO LIST</h3>
          <div v-for="todo in todosAsc" :key="todo.id">
            <div>
              <span>{{ todo.id }}</span>
              <span>{{ todo.todoDescription }}</span>
              <span>{{ todo.todoCategory }}</span>
              <span>{{ todo.date }}</span>
            </div>
          </div>
        </section>
        {{todoCategoryInput}}
      </form>
    </section>
  </section>
</template>

