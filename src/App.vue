<template>
  <div id="app">
    <header>
      <nav class="navbar">
        <div class="logo">
          <img src="./assets/logo.png" alt="To Do logo">
        </div>
      </nav>
    </header>
    <hr>
    <div>
      <h1 class="title">TO DO LIST</h1>
    </div>  
    <hr>
      <br><br><br>
    <div class="container">  
      <div class="input-group">
        <input type="text" class="form-control" placeholder="Add item..." v-model="userInput" @keyup.enter="addItem">
        <button class="btn btn-success" @click="addItem">ADD</button>
      </div>
      <br> 
      <div class="todo-table">
        <div class="table-header">
          <div class="table-cell">Task</div>
          <div class="table-cell">Actions</div>
        </div>
        <div class="table-body">
          <div class="table-row" v-for="(item, index) in filteredList" :key="item.id" :class="{ 'even-row': index % 2 === 0, 'completed': item.completed }">
            <div class="table-cell" :class="{ 'completed-cell': item.completed }">{{ item.value }}</div>
            <div class="table-cell">
              <button class="btn btn-primary" @click="toggleCompleted(index)">
                {{ item.completed ? 'Undo' : 'Complete' }}
              </button>
              <button class="btn btn-info" @click="editItem(index)">Edit</button>
              <button class="btn btn-danger" @click="deleteItem(index)">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      userInput: '',
      searchInput: '',
      list: JSON.parse(localStorage.getItem('todos')) || []
    };
  },
  computed: {
    filteredList() {
      return this.list.filter(item => item.value.toLowerCase().includes(this.searchInput.toLowerCase()));
    }
  },
  methods: {
    saveToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.list));
    },
    addItem() {
      if (this.userInput.trim() !== '') {
        const newItem = {
          id: Math.random(),
          value: this.userInput.trim(),
          completed: false
        };
        this.list.push(newItem);
        this.userInput = '';
        this.saveToLocalStorage();
      }
    },
    deleteItem(index) {
      this.list.splice(index, 1);
      this.saveToLocalStorage();
    },
    editItem(index) {
      const editedTodo = prompt('Edit the todo:', this.list[index].value);
      if (editedTodo !== null && editedTodo.trim() !== '') {
        this.list[index].value = editedTodo.trim();
        this.saveToLocalStorage();
      }
    },
    toggleCompleted(index) {
      this.list[index].completed = !this.list[index].completed;
      this.saveToLocalStorage();
    }
  }
};
</script>

<style src="./style.css"></style>
