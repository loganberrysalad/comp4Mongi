<template>
  <div>
    <button @click="openModal">Open Modal</button>
    <button @click="sortItems">Sort Items</button>
    <table>
      <thead>
        <tr>
          <th>Product Name</th>
          <th>Quantity</th>
          <th>Price</th>
          <th>Category</th>
          <th>Description</th>
          <th>Image</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in items" :key="index">
          <td>{{ item.prodName }}</td>
          <td>{{ item.quantity }}</td>
          <td>{{ item.price }}</td>
          <td>{{ item.category }}</td>
          <td>{{ item.prodDesc }}</td>
          <td>
            <img :src="item.prodUrl" alt="Product Image" width="50" height="50">
          </td>
          <td>
            <button @click="editItem(index)">Edit</button>
            <button @click="deleteItem(index)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="isModalOpen" class="modal">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <form @submit.prevent="addItem">
          <label for="prodName">Product Name:</label>
          <input type="text" v-model="newItem.prodName" required>
          <label for="quantity">Quantity:</label>
          <input type="number" v-model="newItem.quantity" required>
          <label for="price">Price:</label>
          <input type="number" v-model="newItem.price" required>
          <label for="category">Category:</label>
          <input type="text" v-model="newItem.category" required>
          <label for="prodDesc">Description:</label>
          <input type="text" v-model="newItem.prodDesc" required>
          <label for="prodUrl">Image URL:</label>
          <input type="url" v-model="newItem.prodUrl" required>
          <button type="submit">{{ editIndex !== null ? 'Update Item' : 'Add Item' }}</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isModalOpen: false,
      newItem: {
        prodName: "",
        quantity: 0,
        price: 0,
        category: "",
        prodDesc: "",
        prodUrl: ""
      },
      editIndex: null,
      items: []
    };
  },
  methods: {
    openModal() {
      this.isModalOpen = true;
    },
    closeModal() {
      this.isModalOpen = false;
      this.newItem = {
        prodName: "",
        quantity: 0,
        price: 0,
        category: "",
        prodDesc: "",
        prodUrl: ""
      };
      this.editIndex = null;
    },
    editItem(index) {
      const editedItem = this.items[index];
      this.newItem = { ...editedItem };
      this.editIndex = index;
      this.isModalOpen = true;
    },
    deleteItem(index) {
      this.items.splice(index, 1);
      this.saveItemsToLocalStorage();
    },
    sortItems() {
      this.items.sort((a, b) => a.prodName.localeCompare(b.prodName));
      this.saveItemsToLocalStorage();
    },
    addItem() {
      if (this.editIndex !== null) {
        // Edit mode
        this.items[this.editIndex] = { ...this.newItem };
        this.editIndex = null;
      } else {
        // Add mode
        this.items.push({ ...this.newItem });
      }
      this.saveItemsToLocalStorage();
      this.closeModal();
    },
    saveItemsToLocalStorage() {
      localStorage.setItem("items", JSON.stringify(this.items));
    },
    loadItemsFromLocalStorage() {
      const storedItems = localStorage.getItem("items");
      if (storedItems) {
        this.items = JSON.parse(storedItems);
      }
    }
  },
  created() {
    this.loadItemsFromLocalStorage();
  }
};
</script>

<style>
/* Your CSS styles here */
</style>




<style>
/* Styles for the modal */
.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
