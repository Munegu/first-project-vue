<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping List App');
const characterCount = computed(() => {
  return newItem.value.length;
});
const editing = ref(false);
const items = ref([
   {id: 1, label: '10 party hats', purchased: true, highPriority: false},
   {id: 2, label: '2 board games', purchased: true, highPriority: false},
   {id: 3, label: '20 cups', purchased: false, highPriority: true}
]);
const reversedItems = computed(() => {
  return [...items.value].reverse();
});
const newItem = ref("");
const newItemHighPriority = ref(false);
const saveItem= () => {
  items.value.push({id: items.value.length + 1, label: newItem.value, purchased: false, highPriority: newItemHighPriority.value});
  newItem.value = "";
  newItemHighPriority.value = false;
}
const doEdit = () => {
  editing.value = !editing.value;
  newItem.value = "";
  newItemHighPriority.value = false;
}
const togglePurchased = (item) => {
  item.purchased = !item.purchased;
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button class="btn btn-cancel" v-if="editing" @click="doEdit">
      Cancel
    </button>
    <button class="btn btn-primary" v-else @click="doEdit">
      Add
    </button>

  </div>
  <form class="add-item-form" v-if="editing" @submit.prevent="saveItem">

  <input v-model="newItem" type="text" placeholder="Add an item">
  <label>
    <input type="checkbox" v-model="newItemHighPriority">
    High priority
  </label>
  <button class="btn btn-primary" type="submit" :disabled="newItem.length === 0" >
    Save Item
  </button>
  </form>

  <p class="counter" v-if="editing">
    {{ characterCount }}/200
  </p>

  <ul>
    <li v-for="item in reversedItems" @click="togglePurchased(item)" :key="item.id" :class="{strikeout: item.purchased, priority: item.highPriority}">
      {{ item.label }}
    </li>
  </ul>

  <p v-if="0 === items.length">Nothing to see here</p>
</template>