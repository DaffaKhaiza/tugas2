<template>
  <div class="container">
    <h2 style="color: #ff5733;">LIST ORDER MAKANAN</h2>
    <div class="form-group">
      <input type="text" v-model="inputData" placeholder="Masukkan data baru" />
      <button @click="addData" class="custom-button">Tambah</button>
    </div>
    <ul class="data-list">
      <li v-for="(item, index) in data" :key="index" :class="{ 'highlight': index === editIndex }" class="data-item">
        <span v-if="index !== editIndex" style="color: #20c997;">{{ item }}</span>
        <form v-else @submit.prevent="saveEdit(index)">
          <input type="text" v-model="editedData" />
          <button type="submit" class="custom-button custom-button-save">Simpan</button>
          <button @click="cancelEdit" class="custom-button custom-button-cancel">Batal</button>
        </form>
        <div>
          <button @click="editData(index)" v-if="index !== editIndex" class="custom-button custom-button-edit">Edit</button>
          <button @click="deleteData(index)" class="custom-button custom-button-delete">Hapus</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputData: "",
      data: [],
      editIndex: null,
      editedData: "",
    };
  },
  methods: {
    addData() {
      if (this.inputData.trim() !== "") {
        this.data.push(this.inputData);
        this.inputData = "";
        this.saveData();
      }
    },
    editData(index) {
      this.editIndex = index;
      this.editedData = this.data[index];
    },
    saveEdit(index) {
      if (this.editedData.trim() !== "") {
        this.data.splice(index, 1, this.editedData);
        this.editIndex = null;
        this.editedData = "";
        this.saveData();
      }
    },
    cancelEdit() {
      this.editIndex = null;
      this.editedData = "";
    },
    deleteData(index) {
      this.data.splice(index, 1);
      this.saveData();
    },
    saveData() {
      localStorage.setItem("userData", JSON.stringify(this.data));
    },
  },
  mounted() {
    const savedData = localStorage.getItem("userData");
    if (savedData) {
      this.data = JSON.parse(savedData);
    }
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f8f9fa; 
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

input[type="text"] {
  width: calc(100% - 80px);
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.custom-button {
  padding: 10px 20px;
  border: none;
  border-radius: 50px;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
}

.custom-button:hover {
  filter: brightness(80%);
}

.custom-button-save {
  background-color: #28a745; 
}

.custom-button-cancel {
  background-color: #dc3545; 
}

.custom-button-edit {
  background-color: #007bff; 
}

.custom-button-delete {
  background-color: #6c757d; 
}

.data-list {
  list-style-type: none;
  padding: 0;
}

.data-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  border-radius: 8px;
  background-color: #fff;
}

.data-item:last-child {
  border-bottom: none;
}

.highlight {
  background-color: #cce5ff;
}

form {
  display: flex;
  align-items: center;
}
</style>
