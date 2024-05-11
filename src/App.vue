<template>
  <div class="app">
    <h1>To Do List</h1>
    <input type="text" v-model="newItem" placeholder="Ketikkan kegiatan hari ini" @keyup.enter="addItem">
    <p v-if="totalItems === 0" class="empty-message">To-do list masih kosong.</p>
    <p v-else class="total-items">Total {{ totalItems }} kegiatan dalam daftar.</p>
    <ul>
      <li v-for="(item, index) in items" :key="index" class="todo-item">
        <span :class="{ 'completed': item.completed }">{{ item.text }}</span>
        <span class="info">{{ item.updatedInfo }}</span>
        <div class="button-container">
          <button @click="editItem(index)">Edit</button>
          <button @click="toggleCompleted(index)"> {{ item.completed ? 'Undo' : 'Done' }}</button>
          <button @click="deleteItem(index)">Delete</button>
        </div>
        <!-- Tambahkan input untuk pengeditan -->
        <input v-if="item.editing" type="text" v-model="item.updatedText" @keyup.enter="saveEdit(index)" @blur="cancelEdit(index)">
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem: '',
      items: []
    };
  },
  computed: {
    totalItems() {
      return this.items.length;
    }
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        const currentTime = new Date();
        const formattedTime = this.formatTime(currentTime);
        this.items.push({ text: this.newItem, completed: false, updatedInfo: formattedTime });
        this.newItem = '';
      }
    },
    editItem(index) {
      // Tandai item yang diedit
      this.items[index].editing = true;
      // Simpan teks asli untuk pengeditan
      this.items[index].updatedText = this.items[index].text;
    },
    saveEdit(index) {
      // Perbarui teks item dan nonaktifkan mode pengeditan
      if (this.items[index].updatedText.trim() !== '') {
        this.items[index].text = this.items[index].updatedText;
        this.items[index].editing = false;
        const currentTime = new Date();
        const formattedTime = this.formatTime(currentTime);
        this.items[index].updatedInfo = formattedTime;
      }
    },
    cancelEdit(index) {
      // Batal mode pengeditan dan kembalikan teks ke teks asli
      this.items[index].editing = false;
      this.items[index].updatedText = this.items[index].text;
    },
    deleteItem(index) {
      this.items.splice(index, 1);
    },
    toggleCompleted(index) {
      this.items[index].completed = !this.items[index].completed;
    },
    formatTime(time) {
      const hours = time.getHours().toString().padStart(2, '0');
      const minutes = time.getMinutes().toString().padStart(2, '0');
      const day = time.getDate().toString().padStart(2, '0');
      const month = (time.getMonth() + 1).toString().padStart(2, '0');
      const year = time.getFullYear();
      return `${day}-${month}-${year} ${hours}:${minutes}`;
    }
  }
};
</script>

<style scoped>
.app {
  font-family: Arial, sans-serif;
  width: 400px; /* Lebar ditambahkan di sini */
  margin: 0 auto;
  background: linear-gradient(135deg, #f6d365 0%, #fda085 100%);
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.2);
}

.input-container {
  display: flex;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px 0 0 5px;
}

button {
  padding: 10px 20px;
  background-color: #1caa9c;
  border: none;
  color: white;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
}

button:hover {
  background-color: #169c85;
}

.empty-message {
  font-style: italic;
  color: #666;
}

.total-items {
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

.todo-item {
  background: #282722;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 5px;
  box-shadow: 0px 2px 5px rgba(0,0,0,0.1);
}

.completed {
  text-decoration: line-through;
  color: #999;
}

.info {
  margin-left: 10px;
  font-size: 12px;
  color: #666;
}

.button-container {
  margin-top: 10px;
}

.button-container button {
  margin-right: 5px;
}
</style>
