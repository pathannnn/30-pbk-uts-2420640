<template>
    <Menu />
    <section class="todo-list">
        <h1>Todo List</h1>
        <div>
            <input v-model="newTodo" placeholder="Add new todo" />
            <button @click="addTodo">Add</button>
        </div>
        <ul>
            <li v-for="(todo, index) in todos" :key="index">
                <input type="checkbox" v-model="todo.completed" />
                <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
                <button @click="removeTodo(index)">Delete</button>
            </li>
        </ul>
        <p>{{ remaining }} remaining out of {{ todos.length }} tasks</p>
    </section>
</template>

<script>
import Menu from '../components/Menu.vue'
export default {
    components: { Menu },
    data() {
        return {
            newTodo: '',
            todos: [
            ]
        }
    },
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length;
        }
    },
    methods: {
        addTodo() {
            if (this.newTodo.trim() !== '') {
                this.todos.push({ text: this.newTodo, completed: false });
                this.newTodo = '';
            }
        },
        removeTodo(index) {
            this.todos.splice(index, 1);
        }
    }
}
</script>

<style>
.todo-list {
    max-width: 400px;
    margin: 0 auto;
    padding: 1rem;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.todo-list h1 {
    font-size: 3.5rem;
    text-align: center;
    margin-bottom: 1rem;
}
.todo-list input[type="text"] {
    width: calc(100% - 60px);
    padding: 0.5rem;
    margin-right: 10px;
}
.todo-list button {
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
.todo-list button:disabled {
    background-color: #ccc;
}
.todo-list ul {
    list-style: none;
    padding: 0;
}
.todo-list li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.5rem 0;
    border-bottom: 1px solid #ddd;
}
.todo-list li:last-child {
    border-bottom: none;
}
.todo-list li span.completed {
    text-decoration: line-through;
    color: #888;
}
.todo-list li button {
    background-color: #dc3545;
}
.todo-list li input[type="checkbox"] {
    margin-right: 10px;
}
</style>
