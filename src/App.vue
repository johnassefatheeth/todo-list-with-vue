<script setup>
import { ref,onMounted, computed , watch} from 'vue'

const todos= ref([])
const name =ref('')

const input_content= ref('')
const input_category = ref(null)

const todos_asc = computed(()=> todos.value.sort((a,b)=>{
  return b.createdAt = a.createdAt
}))

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
</main>
</template>

