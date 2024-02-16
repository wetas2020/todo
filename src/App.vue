<script setup>
import { ref, watch, onMounted } from 'vue'

var newTodo = ref('')
var todosList = ref([])

var addTodo = () => {
  todosList.value.push({
    title: newTodo.value,
    done: false
  })
  newTodo.value = ''
  console.log(todosList.value)
}

var markDone = (todo) => {
  todo.done = true
}

var deleteTodo = (todo) => {
  todosList.value = todosList.value.filter((t) => t !== todo)
}

watch(
  todosList,
  (newTodos) => {
    localStorage.setItem('todos', JSON.stringify(newTodos))
  },
  { deep: true }
)

onMounted(() => {
  if (localStorage.getItem('todos')) {
    todosList.value = JSON.parse(localStorage.getItem('todos'))
  }
})
</script>

<template>
  <div class="container">
    <h1 class="text-center">Todo App</h1>
    <form @submit.prevent="addTodo()">
      <div class="form-group">
        <label for="newTodo">New Todo</label>
        <input
          v-model="newTodo"
          id="newTodo"
          name="newTodo"
          type="text"
          class="form-control"
          placeholder="Walk the dog..."
          aria-describedby="newTodoHelp"
        />
        <small id="newTodoHelp" class="form-text text-muted">Enter a new todo.</small>
      </div>
      <input type="submit" class="btn btn-primary mt-3" value="Add Todo" />
    </form>
    <ul class="list-group mt-3">
      <li v-for="(todo, index) in todosList" :key="index" class="list-group-item">
        <span :class="{ isDone: todo.done }">{{ todo.title }}</span>
        <button
          v-if="!todo.done"
          @click="markDone(todo)"
          type="button"
          class="btn btn-primary mx-2"
        >
          Done
        </button>
        <button @click="deleteTodo(todo)" type="button" class="btn btn-danger mx-2">Delete</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.isDone {
  text-decoration: line-through;
}
</style>
