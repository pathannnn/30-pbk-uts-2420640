<template>
    <q-header>
      <q-toolbar>
        <q-toolbar-title>Fatih Masrukhan</q-toolbar-title>
        <q-btn flat @click="navigate('/todos')">Todos</q-btn>
        <q-btn flat @click="navigate('/posts')">Posts</q-btn>
        <q-btn flat @click="navigate('/albums')">Albums</q-btn>
      </q-toolbar>
    </q-header>
  </template>
  
  <script>
  import { useRouter } from 'vue-router'
  
  export default {
    setup() {
      const router = useRouter()
      const navigate = (path) => {
        router.push(path)
      }
      return { navigate }
    }
  }
  </script>
  
  <style scoped>
  </style>
  