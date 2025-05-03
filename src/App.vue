<script setup>
import { ref } from 'vue'

const items = ref([])
const newItem = ref('')

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

</script>

<template>
  <input type="text" v-model="newItem" @keyup.enter="addItem" />
  <button @click="addItem">Tambahkan</button>

  <ul>
    <li v-for="item in items" :key="item.id">
      {{  item.text }}
    </li>
  </ul>
</template>

<style scoped></style>
