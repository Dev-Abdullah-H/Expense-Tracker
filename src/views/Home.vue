<template>
  <div class="container">
    <div class="content">
      <h1>Expense Tracker</h1>
      <h6>Made by - Muhammad Abdullah Haider</h6>
      <div class="form-container">
        <div class="input-row">
          <input
            v-model="newItem.name"
            type="text"
            placeholder="Enter new item..."
          />
          <input
            v-model="newItem.price"
            type="text"
            placeholder="Enter price..."
          />
        </div>
        <div class="button-row">
          <button @click="addItem">Add New Item</button>
          <button @click="clearAll">Clear All</button>
        </div>
      </div>
      <div class="items-container" v-if="items.length != 0">
        <div v-for="(item, index) in items" :key="index" class="item-row">
          <h3>
            <strong>{{ item.name }} : </strong>${{ item.price }}
          </h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const newItem = ref({ name: "", price: "" });
const items = ref([]);

const loadItems = () => {
  const savedItems = JSON.parse(localStorage.getItem("items")) || [];
  items.value = savedItems;
};

const addItem = () => {
  if (newItem.value.name && newItem.value.price) {
    items.value.push({ ...newItem.value });
    localStorage.setItem("items", JSON.stringify(items.value));
    newItem.value.name = "";
    newItem.value.price = "";
  } else {
    alert("Name or Price is missing! please fill it.");
  }
};

const clearAll = () => {
  items.value = [];
  localStorage.removeItem("items");
};
onMounted(() => {
  loadItems();
});
</script>

<style scoped>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f0f2f5;
}

.container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  background-color: #f0f2f5;
  padding: 20px;
}

.content {
  width: 100%;
  max-width: 500px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-top: 20px;
}

h1, h6 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.form-container {
  margin-bottom: 20px;
}

.input-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

input {
  margin: 0 0.5vw;
  outline: none;
}

input[type="text"] {
  width: 48%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
}

.button-row {
  display: flex;
  justify-content: space-between;
  margin: 1vw;
}

button {
  width: 48%;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

.items-container {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.item-row {
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.item-row:last-child {
  border-bottom: none;
}

strong {
  color: #333;
}

h3 {
  margin: 0;
  color: #555;
}
</style>