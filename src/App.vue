<script setup>
//setting up the local storage
import { ref, onMounted, computed, watch } from 'vue'

//setup the global variables
const todos = ([]) 
const name = ref ('')

const input_content = ref('')
const input_category = ref(null)

//filtering system setup
const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return  
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>


<template>

  <main class="app">

    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" v-model="name"/>
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TASK</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your list today?</h4>
        <input 
          type="text" 
          placeholder="e.g. make some money" 
          v-model="input_content" />
          <h4>Pick a category</h4>

          <div class="options">
            <label>
              <input 
                type="radio" 
                name="category"
                value="business"
                v-model="input_category" />
                <span class="bubble business"></span>
                <div>Business</div>
            </label>

            <label>
              <input 
                type="radio" 
                name="category"
                value="personal"
                v-model="input_category" />
                <span class="bubble personal"></span>
                <div>Personal</div>
            </label>
          </div>

          <input type="submit" value="Add Task" />
      </form>
    </section>

  </main>

</template>
