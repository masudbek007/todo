<template>
  <div class="max-w-md mx-auto mt-10">
    <h1 class="text-2xl font-bold mb-4">TODO List</h1>
    <div class="flex mb-4">
      <input
        v-model="newTodo"
        class="border p-2 w-full"
        placeholder="Add a new task"
      />
      <button @click="addTodo" class="bg-green-500 text-white px-4 py-2 rounded ml-2">Add</button>
    </div>
    <ul>
      <li
        v-for="(todo, index) in todos"
        :key="index"
        class="flex justify-between items-center mb-2 p-2 border rounded"
      >
        <div class="flex-grow cursor-pointer" @click="openEditModal(index)">
          <span :class="{ 'line-through': todo.completed }">{{ todo.text }}</span>
          <div class="text-sm text-gray-500">{{ formatDate(todo.timestamp) }}</div>
        </div>
        <button @click="toggleComplete(index)" class="bg-blue-500 text-white px-2 py-1 rounded">
          {{ todo.completed ? 'Undo' : 'Complete' }}
        </button>
        <button @click="removeTodo(index)" class="bg-red-500 text-white px-2 py-1 rounded ml-2">
          Delete
        </button>
      </li>
    </ul>

    <div v-if="showEditModal" class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center">
      <div class="bg-white p-4 rounded shadow-lg max-w-md w-full">
        <h2 class="text-xl font-bold mb-4">Edit Task</h2>
        <input
          v-model="editTodoText"
          class="border rounded-xl p-2 w-full mb-4"
          placeholder="Edit task"
        />
        <div class="flex justify-end">
          <button @click="saveEditTodo" class="bg-green-500 text-white px-4 py-2 rounded mr-2">Save</button>
          <button @click="hideEditModal" class="bg-red-500 text-white px-4 py-2 rounded">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      showEditModal: false,
      currentEditIndex: null,
      editTodoText: ''
    };
    
  },
  
  methods: {
    
    addTodo() {
        if (this.newTodo.trim()) {
        this.todos.push({
          text: this.newTodo.trim(),
          completed: false,
          timestamp: new Date(),
          editing: false
        });
        this.newTodo = '';
      }
    },
    toggleComplete(index) {
      this.todos[index].completed = !this.todos[index].completed;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    openEditModal(index) {
      this.currentEditIndex = index;
      this.editTodoText = this.todos[index].text;
      this.showEditModal = true;
    },
    saveEditTodo() {
      if (this.editTodoText.trim()) {
        this.todos[this.currentEditIndex].text = this.editTodoText.trim();
        this.showEditModal = false;
      }
    },
    hideEditModal() {
      this.showEditModal = false;
    },
    formatDate(date) {
      const options = {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit',
      };
      return new Date(date).toLocaleDateString(undefined, options);
    }
  }
}
</script>

<style scoped>
.line-through {
  text-decoration: line-through;
}
</style>
