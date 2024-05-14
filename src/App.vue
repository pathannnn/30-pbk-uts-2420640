<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')
const input_content = ref('')
const post_title = ref('')

const todos_asc = computed(() => todos.value.sort((a,b) =>{
    return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
    localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
}, {
    deep: true
})

const addTodo = () => {
    if (input_content.value.trim() === '') {
        return
    }

    todos.value.push({
        content: input_content.value,
        done: false,
        editable: false,
        createdAt: new Date().getTime()
    })
    input_content.value = ''
}

const addPost = () => {
    // Fetch data from the dummy database
    fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => {
            // Transform fetched data to match your todo structure
            const posts = data.map(user => ({
                content: `${user.name}`, // 
                done: false,
                editable: false,
                createdAt: new Date().getTime() // You can set createdAt to any appropriate value
            }));
            
            // Add fetched posts to todos
            todos.value = [...todos.value, ...posts];
        })
        .catch(error => {
            console.error('Error fetching posts:', error);
        });

    post_title.value = ''
}



const removeTodo = (todo) => {
    todos.value = todos.value.filter((t) => t !== todo)
}

const toggleLineThrough = (todo) => {
    todo.done = !todo.done
}


</script>

  
<template>
    <main class="app">
        <section class="greeting">
            <h2 class="title">
                What's up, <input type="text" id="name" placeholder="Name here" v-model="name">
            </h2>
        </section>

        <section class="create-todo">
            <h3>CREATE A TODO</h3>

            <form id="new-todo-form" @submit.prevent="addTodo">
                <h4>What's on your todo list?</h4>
                <input 
                    type="text" 
                    name="content" 
                    id="content" 
                    placeholder="Write your todo"
                    v-model="input_content" />
                <input type="submit" value="Add todo" />
            </form>
        </section>

        <section class="create-post">
            <form id="new-post-form" @submit.prevent="addPost">
                <h4>Post Title</h4>
                <input 
                    type="text" 
                    name="title" 
                    id="post-title" 
                    placeholder="Post title"
                    v-model="post_title" />
                <input type="submit" value="Add post" />
            </form>
        </section>

        <section class="todo-list">
            <h3>Todo List</h3>
            <div class="list" id="todo-list">
                <div 
                    v-for="todo in todos_asc" 
                    :key="todo.createdAt" 
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
    </main>
</template>

<style scoped>

.line-through {
    text-decoration: line-through;
}
</style>




