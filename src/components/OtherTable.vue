<template>
  <div v-if="users.length" class="my-3">
    [OtherTables.vue]This is an other component that loads data from store, i.e. without props from parents.
    <br />
    [UserRowNoClone.vue] Data in this table loads items.
    <table class="table w-full table-zebra">
      <thead>
        <tr>
          <th></th>
          <th>Name</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <UserRowNoClone v-for="user in users" :key="user.id" :user="user" />
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { useFind } from 'feathers-pinia'
import { useUserStore } from '~/stores/user'

const userStore = useUserStore()
const User = userStore.Model

const params = computed(() => {
  return { query: {} }
})
const { items: users } = useFind({ model: User, params })
</script>
