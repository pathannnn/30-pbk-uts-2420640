<template>
    <Menu />
    <section class="create-post">
        <h1>Create Post</h1>
        <div>
            <input v-model="query" @input="fetchUsers" placeholder="Type to search users" />
            <ul v-if="filteredUsers.length">
                <li v-for="user in filteredUsers" :key="user.id" @click="selectUser(user)">
                    {{ user.name }}
                </li>
            </ul>
        </div>
        <div v-if="selectedUsers.length">
            <h2>Users</h2>
            <ul>
                <li v-for="user in selectedUsers" :key="user.id">
                    {{ user.name }}
                    <button @click="removeUser(user.id)">Remove</button>
                </li>
            </ul>
        </div>
    </section>
</template>

<script>
import Menu from '../components/Menu.vue'
export default {
    components: { Menu },
    data() {
        return {
            query: '',
            users: [],
            filteredUsers: [],
            selectedUsers: []
        };
    },
    methods: {
        fetchUsers() {
            if (this.query.trim() === '') {
                this.filteredUsers = [];
                return;
            }
            fetch('https://jsonplaceholder.typicode.com/users')
                .then(response => response.json())
                .then(data => {
                    this.users = data;
                    this.filteredUsers = this.users.filter(user =>
                        user.name.toLowerCase().includes(this.query.toLowerCase())
                    );
                });
        },
        selectUser(user) {
            if (!this.selectedUsers.includes(user)) {
                this.selectedUsers.push(user);
                this.query = '';
                this.filteredUsers = [];
            }
        },
        removeUser(userId) {
            this.selectedUsers = this.selectedUsers.filter(user => user.id !== userId);
        }
    }
};
</script>

<style>
.create-post {
    max-width: 400px;
    margin: 0 auto;
    padding: 1rem;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
h2{
    font-size: 2rem;
    margin-bottom: 0.1rem;
}
.create-post h1 {
    font-size: 3rem;
    text-align: center;
    margin-bottom: 1rem;
}
.create-post input[type="text"] {
    width: calc(100% - 20px);
    padding: 0.5rem;
    margin-bottom: 0.5rem;
}
.create-post ul {
    list-style: none;
    padding: 0;
}
.create-post li {
    padding: 0.5rem 0;
    border-bottom: 1px solid #ddd;
    cursor: pointer;
}
.create-post li:last-child {
    border-bottom: none;
}
.create-post li:hover {
    background-color: #f0f0f0;
}
.create-post button {
    margin-left: 10px;
    padding: 0.3rem 0.6rem;
    background-color: #dc3545;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
</style>
