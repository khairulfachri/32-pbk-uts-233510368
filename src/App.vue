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

const remove = (item) => {
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
  <div class="min-h-screen bg-gradient-to-br from-teal-50 via-cyan-100 to-white flex items-center justify-center p-6">
    <div class="w-full max-w-6xl bg-white rounded-2xl shadow-lg flex overflow-hidden">

      <!-- Tugas -->
      <div class="w-2/3 p-6 overflow-y-auto">
        <h2 class="text-2xl font-bold text-gray-700 mb-4">Daftar Tugas</h2>
        <ul class="space-y-4">
          <li
            v-for="item in filteredItems"
            :key="item.id"
            class="flex justify-between items-center bg-cyan-50 border border-cyan-200 rounded-lg px-4 py-3 shadow-sm"
          >
            <div class="flex items-center gap-3">
              <input
                type="checkbox"
                v-model="item.completed"
                @change="toggle(item)"
                class="accent-teal-600"
              />
              <span :class="{ 'line-through text-gray-400': item.completed }">{{ item.text }}</span>
            </div>
            <button @click="remove(item)" class="text-sm text-red-500 hover:underline">
              Hapus
            </button>
          </li>
        </ul>
      </div>

      <!-- Sidebar Form (kanan) -->
      <div class="w-1/3 bg-gradient-to-t from-cyan-200 to-teal-300 p-6 text-gray-800 flex flex-col justify-between">
        <div>
          <h2 class="text-xl font-bold mb-4 text-white">Tambah Tugas</h2>
          <input
            type="text"
            v-model="newItem"
            @keyup.enter="addItem"
            placeholder="Tugas baru..."
            class="w-full px-4 py-2 rounded-md mb-3 text-gray-700 focus:outline-none focus:ring-2 focus:ring-teal-400"
          />
          <button
            @click="addItem"
            class="w-full bg-white text-teal-700 font-semibold px-4 py-2 rounded-md hover:bg-gray-100 transition"
          >
            Tambahkan
          </button>
        </div>

        <div class="mt-8">
          <label class="block mb-2 text-white font-medium">Filter Tugas</label>
          <select
            v-model="filter"
            class="w-full px-4 py-2 rounded-md text-teal-800 focus:outline-none"
          >
            <option value="all">Semua</option>
            <option value="completed">Selesai</option>
            <option value="uncompleted">Belum Selesai</option>
          </select>
        </div>
      </div>

    </div>
  </div>
</template>

<style scoped></style>
