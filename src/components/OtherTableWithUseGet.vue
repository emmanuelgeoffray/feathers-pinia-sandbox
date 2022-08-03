<template>
  <div v-if="user" class="my-3">
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
        <UserRowNoClone :key="user.id" :user="user" />
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import { useGet } from 'feathers-pinia'
import { useUserStore } from '~/stores/user'

const userStore = useUserStore()
const User = userStore.Model

const { item: user } = useGet({ model: User, id: 0 })
</script>
