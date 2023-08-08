<script setup>
import { ref,onMounted, computed , watch} from 'vue'

const todos= ref([])
const name =ref('')

const input_content= ref('')
const input_category = ref(null)

const todos_asc = computed(()=> todos.value.sort((a,b)=>{
  return b.createdAt - a.createdAt
}))

const deletetodo = (todo) =>{
  todos.value = todos.value.filter(t => t !== todo)
}

const addtodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }
  const newTodo = {
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  }
  todos.value.push(newTodo)
  localStorage.setItem('todos', JSON.stringify(todos.value))
}

watch(todos ,(newTodo)=>{
  localStorage.setItem('todos',JSON.stringify(newTodo))
})

watch(name ,(newName)=>{
  localStorage.setItem('name',newName)
},{deep:true})

onMounted(()=>{
  name.value= localStorage.getItem('name') || ''
  const storedTodos = JSON.parse(localStorage.getItem('todos'))
  todos.value = Array.isArray(storedTodos) ? storedTodos : []
})


</script>

<template>
<main class="app">
  <section class="greeting">
    <h2 class="title">
      hello there, <input type="text" placeholder="your name here" v-model="name">
    </h2>
  </section>
  <section class="create-todo">
    <h3>Create a ToDo</h3>

    <form @submit.prevent="addtodo">
      <h4>what do you want on your list?</h4>
      
      <input type="text" 
        placeholder="e.g wash cloths" 
        v-model="input_content">
      <h4>what catagory does it belong to</h4>
      <div class="options">
        <label>
          <input type="radio" 
          name="category"
          value="business"
          v-model="input_category">
          <span class="bubble business"></span>
          <div>Business</div>
        </label>
        <label>
          <input type="radio" 
          name="category"
          value="personal"
          v-model="input_category">
          <span class="bubble personal"></span>
          <div>Personal</div>
        </label>
      </div>
      <input type="submit" value="add todo">
    </form>
  </section>
  <section class="todo-list">
    <h3>YOUR TODO LIST ARE AS FOLLOWS</h3>
    <div class="list">
      <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">      
      
        <label>
          <input type="checkbox" v-model="todo.done">
          <span :class="`bubble ${todo.category=='business'?'business':'personal'}`"></span>
        </label>

        <div class="todo-content">
          <input type="text" v-model="todo.content">
        </div>

        <div class="actions">
          <button class="delete" @click="deletetodo(todo)">delete</button>
        </div>
      
      </div>


    </div>
  </section>
</main>
</template>

