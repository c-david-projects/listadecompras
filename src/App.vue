<template>
  <div class="container" style="margin-top:7.5em;">
    <div>
    <!-- UI for creating new items -->
    <h2>Lista de compras</h2>
    <form @submit.prevent="createItem">
      <div class="form-group" style="margin-top: 1em;
    margin-bottom: 2em;">
        <label for="item-name">Item</label>
        <input type="text" class="form-control" id="item-name" v-model="newItem.name" required>
      </div>
      <div class="form-group" style="margin-top: 1em;
    margin-bottom: 2em;">
        <label for="item-price">Preço</label>
        <input type="number" step="0.01" class="form-control" id="item-price" v-model.number="newItem.price" required>
      </div>
      <div class="btn-container">
      <button type="submit" class="btn btn-primary">Criar</button>
      </div>
    </form>

    <!-- UI for displaying existing items -->
    <div style="margin-top: 1em;
    margin-bottom: 2em;">
    <h2>Items existentes</h2>
    <ul class="list-group">
      <li v-for="(item, index) in items" :key="item.id">
        <div>
          <strong>{{ item.name }}</strong> - €{{ item.price.toFixed(2) }}
          <div style="display:flex;justify-content:end;">
          <button class="btn btn-danger" @click="deleteItem(index)">Apagar</button>
          </div>
        </div>
        <hr>
      </li>
    </ul>
  </div>

    <!-- UI for displaying total price of all items -->
    <h2>Total</h2>
    <p>€{{ total.toFixed(2) }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Initialize component data with an empty array for items
      items: [],
      // Initialize an empty object for creating new items
      newItem: {
        id: null,
        name: "",
        price: null
      }
    };
  },
  // Use the created() lifecycle hook to retrieve existing items from localStorage
  created() {
    const savedItems = localStorage.getItem("items");
    if (savedItems) {
      this.items = JSON.parse(savedItems);
    }
  },
  computed: {
    // Compute the total price of all items
    total() {
      return this.items.reduce((sum, item) => sum + parseFloat(item.price), 0);
    }
  },
  methods: {
    // Method to create a new item
    createItem() {
      // Generate a new ID for the item
      const newItemId = Date.now();
      // Set the ID of the new item and add it to the items array
      this.newItem.id = newItemId;
      this.items.push(this.newItem);
      // Save the updated items array to localStorage
      localStorage.setItem("items", JSON.stringify(this.items));
      // Reset the newItem object for creating new items
      this.newItem = {
        id: null,
        name: "",
        price: null
      };
    },
    // Method to delete an existing item
    deleteItem(index) {
      // Remove the item at the specified index from the items array
      this.items.splice(index, 1);
      // Save the updated items array to localStorage
      localStorage.setItem("items", JSON.stringify(this.items));
    }
  }
};
</script>
