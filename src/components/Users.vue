<script setup>
import { useQuery } from '@tanstack/vue-query';
import { getCurrentInstance, ref } from 'vue';

const currentInstance = getCurrentInstance()

async function getUsers() {
  const res = await fetch('https://jsonplaceholder.typicode.com/users')
  if(!res.ok) {
    throw new Error('Not able to fetch users...')
  }
  const usersData = await res.json()
  console.log(usersData)
  return {...usersData}
}

const { isPending, isFetching, isError, data, error, status } = useQuery({
  queryKey: ['users'],
  queryFn: getUsers,
})
const msg = ref('Hello World!')
</script>

<template>
  <h1>{{ msg }}</h1>
  <input v-model="msg" />

  <h2>Users list</h2>
  <div v-if="isPending || isFetching">
    Fetching users list...
  </div>
  <div v-if="isError">
    {{error.message}}
  </div>
  <ul v-else>
    <li v-for="(user, index) in data" :key="index">
      {{user.name}}
    </li>
  </ul>
</template>
