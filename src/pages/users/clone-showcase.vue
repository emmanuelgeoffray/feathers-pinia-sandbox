<template>
  <!--<div class="overflow-x-auto">-->
  <div>
    <div>
      This page showcases the use of the clone pattern in feathers-pinia.
      <br />
      A clone is a copy of an item from store, but it is not synced. That means any change from server will not be seen on the clone.
      <br />
      Hit button bellow to notice that.
    </div>
    <button type="button" class="btn mt-4 mb-2" @click="patchFirstUser">Mock patch from server</button>

    <div v-if="users.length" class="my-3">
      [UserRow.vue] Data in this table loads clones. An update from server won't be shown here.
      <table class="table w-full table-zebra">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <UserRow v-for="user in users" :key="user.id" :user="user" />
        </tbody>
      </table>
    </div>
    <div v-if="users.length" class="my-3">
      [UserRowNoClone.vue] Data in this table loads items. An update from server will show here.
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

    <div v-else class="card bg-base-300">
      <div class="card-body">
        <div class="card-title">No Users</div>

        <router-link to="/users/new" class="btn w-48 m-auto">
          <feather-plus class="mr-1" />
          Create a User
        </router-link>
      </div>
    </div>
    <!--<UserQuickCreate />-->
  </div>
</template>

<script setup lang="ts">
import { useFind } from 'feathers-pinia'
import { useUserStore } from '~/stores/user'
import { api } from '~/feathers'

const userStore = useUserStore()

// You can find User on the userStore's `Model` property OR...
const User = userStore.Model

// Since we called `useUserStore()` the model is also available on the `models` object.
// import { models, useFind } from 'feathers-pinia'
// const { User } = models.api

const params = computed(() => {
  return { query: {} }
})
const { items: users } = useFind({ model: User, params })

function patchFirstUser() {
  api.service('users').patch('0', { name: 'Emmanuel' })
}
</script>
