<script setup>
import { ref,onMounted, computed , watch} from 'vue'

const todos= ref([])
const name =ref('')

const input_content= ref('')
const input_category = ref(null)

const todos_asc = computed(()=> todos.value.sort((a,b)=>{
  return b.createdAt = a.createdAt
}))

const addtodo = ()=> {
  if(input_content.value.trim() === '' || input_category.value === null){
    return
  }

  todos.value.push({
    content: input_content.value,
    category:input_category,
    done:false,
    createdAt:new Date.getTime()

  })
}

watch(name ,(newName)=>{
  localStorage.setItem('name',newName)
})

onMounted(()=>{
  name.value= localStorage.getItem('name') || ''
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
    <h3>create a ToDo</h3>

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
          <div>personal</div>
        </label>
      </div>
      <input type="submit" value="add todo">
    </form>
  </section>
</main>
</template>

