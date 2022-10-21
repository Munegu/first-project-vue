<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App');
const editing = ref(false);
const items = ref([
  // {id: 1, label: '10 party hats'},
  // {id: 2, label: '2 board games'},
  // {id: 3, label: '20 cups'}
]);
const newItem = ref("");
const newItemHighPriority = ref(false);
const saveItem= () => {
  items.value.push({id: items.value.length + 1, label: newItem.value});
  newItem.value = "";
}
const doEdit = () => {
  editing.value = !editing.value;
  newItem.value = "";
}
// const iceCreamFlavors = ref([]);
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

  <ul>
    <li v-for="({id, label}) in items" :key="id">
      {{ label }}
    </li>
  </ul>

  <p v-if="0 === items.length">Nothing to see here</p>
</template>