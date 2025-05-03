<script setup>
import { ref, computed } from 'vue'

const items = ref([])
const newItem = ref('')
const filter = ref('all')

const addItem = () => {
  if (newItem.value.trim() !== '') {
    items.value.push({
      id: Date.now(),
      text: newItem.value,
      completed: false
    })
    newItem.value = ''
  }
}

const toggle = (item) => {
  item.completed == !item.completed
}

const remove = (item) =>{
  items.value = items.value.filter(i => i.id !== item.id)
}

const filteredItems = computed(() => {
  if (filter.value === 'all') {
    return items.value
  } else if (filter.value === 'completed') {
    return items.value.filter(item => item.completed)
  } else if (filter.value === 'uncompleted') {
    return items.value.filter(item => !item.completed)
  }
})

</script>

<template>
  <input type="text" v-model="newItem" @keyup.enter="addItem" />
  <button @click="addItem">Tambahkan</button>
  <select v-model="filter">
    <option value="all">Semua</option>
    <option value="completed">Selesai</option>
    <option value="uncompleted">Belum Selesai</option>
  </select>

  <ul>
    <li v-for="item in filteredItems" :key="item.id">
      <input type="checkbox" v-model="item.completed" @change="toggle(item)" />
      {{ item.text }}
      <button @click="remove(item)">Hapus</button>
    </li>
  </ul>
</template>

<style scoped></style>
