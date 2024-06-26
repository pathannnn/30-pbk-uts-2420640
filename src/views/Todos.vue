
<!-- views/Todos.vue -->
<template>
    <Menu />
    <section class="todo-list">
        <!-- Existing Todos Template Code -->
    </section>
</template>
<script>

export default {
    components: { Menu }
    // Existing Todos Script Code
}
</script>


<template>
    <section class="todo-list">
      <h3>List</h3>
      <div class="list" id="todo-list">
        <div 
          v-for="todo in todos_asc" 
          :key="todo.id" 
          class="todo-item"
          :class="{ done: todo.done }"
          @click="toggleLineThrough(todo)">
          <label>
            <input type="checkbox" v-model="todo.done" />
          </label>
  
          <div class="todo-content" :class="{ 'line-through': todo.done }">
            {{ todo.content }}
          </div>
  
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </template>
  
  <script setup>
  import { ref, computed, watch, onMounted } from 'vue';
  import Menu from '../components/Menu.vue'
  
  const todos = ref([]);
  const name = ref('');
  const input_content = ref('');
  const users = ref([]);
  const filtered_users = ref([]);
  const show_dropdown = ref(false);
  const selected_user = ref(null);
  
  const todos_asc = computed(() => {
    return todos.value.slice().sort((a, b) => a.createdAt - b.createdAt);
  });
  
  watch(name, (newVal) => {
    localStorage.setItem('name', newVal);
  });
  
  watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal));
  }, {
    deep: true
  });
  
  const addTodo = () => {
    if (input_content.value.trim() === '') {
      return;
    }
  
    todos.value.push({
      id: Date.now(),
      content: input_content.value,
      done: false,
      editable: false,
      createdAt: new Date().getTime()
    });
    input_content.value = '';
  };
  
  const removeTodo = (todo) => {
    todos.value = todos.value.filter((t) => t.id !== todo.id);
  };
  
  const toggleLineThrough = (todo) => {
    todo.done = !todo.done;
  };
  
  onMounted(() => {
    name.value = localStorage.getItem('name') || '';
    const savedTodos = localStorage.getItem('todos');
    if (savedTodos) {
      todos.value = JSON.parse(savedTodos);
    }
  });
  
  </script>
  
  <style scoped>
  .line-through {
    text-decoration: line-through;
  }
  
  .todo-item {
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    display: flex;
    align-items: center;
  }
  
  .todo-item.done {
    opacity: 0.6;
  }
  
  .todo-item .todo-content {
    flex: 1;
    margin-left: 10px;
  }
  
  .todo-item .actions {
    margin-left: auto;
  }
  
  .todo-item .delete {
    background-color: #f44336;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
  }
  
  .todo-item .delete:hover {
    background-color: #d32f2f;
  }
  </style>
  