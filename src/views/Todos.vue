<!-- views/Todos.vue -->
<template>
    <div>
        <Menu />
        <section class="todo-list">
            <!-- Your existing Todo List template code goes here -->
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
    </div>
</template>

<script>
import Menu from '../components/Menu.vue';
import { ref, computed, watch, onMounted } from 'vue';

export default {
    components: {
        Menu
    },
    setup() {
        const todos = ref([]);
        const name = ref('');
        const input_content = ref('');

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

        return {
            todos,
            name,
            input_content,
            todos_asc,
            addTodo,
            removeTodo,
            toggleLineThrough
        };
    }
};
</script>
