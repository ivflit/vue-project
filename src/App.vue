<script setup>
// @ts-nocheck
import { ref, computed } from 'vue'

const header = ref('Shopping List App')
const editing = ref(false)
const items = ref([
   { id: 1, label: "10 party hats", purchased: true, highPriority: false },
   { id: 2, label: "2 board games", purchased: true, highPriority: false },
   { id: 3, label: "20 cups", purchased: false, highPriority: true }
])
const reversedItems = computed(()=>[...items.value].reverse())
const newItem = ref("")
const newItemHighPriority = ref(false)
const saveItem = () => {
	items.value.push({
      id: items.value.length + 1,
      label: newItem.value,
      highPriority: newItemHighPriority.value
    })
  newItem.value = ""
  newItemHighPriority.value = false
}
const doEdit = (e) => {
  editing.value = e
  newItem.value = ""
  newItemHighPriority.value = false
}
const togglePurchased = (item) => {
  item.purchased = !item.purchased
}
</script>

<template>
  <div id="app">
    <section class="section">
      <div class="container">
        <div class="header">
          <h1 class="title">{{ header }}</h1>
          <button 
            v-if="editing" 
            class="button is-danger is-light"
            @click="doEdit(false)"
          >
            Cancel
          </button>
          <button 
            v-else 
            class="button is-primary"
            @click="doEdit(true)"
          >
            Add Item
          </button>
        </div>
        <form 
          v-if="editing"
          class="add-item-form mt-4"
          @submit.prevent="saveItem"
        >
          <div class="field">
            <div class="control">
              <input 
                class="input" 
                type="text" 
                v-model.trim="newItem" 
                placeholder="Add an item"
              >
            </div>
          </div>
          <div class="field">
            <label class="checkbox">
              <input 
                type="checkbox" 
                v-model="newItemHighPriority"
              >
              High Priority
            </label>
          </div>
          <button 
            class="button is-primary" 
            :disabled="newItem.length < 5"
          >
            Save Item
          </button>
        </form>
        <ul class="mt-4">
          <li 
            v-for="(item, index) in reversedItems" 
            :key="item.id"
            class="box mb-2 is-clickable"
            :class="{ 'has-text-grey-light': item.purchased, 'has-text-danger': item.highPriority }"
            @click="togglePurchased(item)"
          >
            <span :class="{ 'is-line-through': item.purchased }">
              {{ item.label }}
            </span>
          </li>
        </ul>
        <p v-if="!items.length" class="has-text-grey">
          Nothing to see here
        </p>
      </div>
    </section>
  </div>
</template>


<style scoped>
/* Additional custom styles */
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.add-item-form {
  display: flex;
  align-items: center;
  gap: 1rem;
}
.is-line-through {
  text-decoration: line-through;
}
</style>
