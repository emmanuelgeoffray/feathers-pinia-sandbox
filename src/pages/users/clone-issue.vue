<template>
  <!--<div class="overflow-x-auto">-->
  <div>
    <div>
      This page demonstrates an
      <u><a href="https://github.com/marshallswain/feathers-pinia/issues/52"> issue </a></u>
      with clones currently with feathers-pinia. A quick fix is to clear existing clone.
      <br />
      To reproduce:
      <ul>
        <li>1. Modify input below</li>
        <li>2. Hit button below</li>
        <li>3. Modify input again to trigger errors.</li>
      </ul>
    </div>
    <button type="button" class="btn mt-4 mb-2" @click="patchFirstUser">Mock patch from server</button>

    <div v-if="users.length" class="my-3">
      [UserRowCloneAtSaveWithoutFix.vue] Triggers <u>errors</u>. Data in this table loads items. An update from server will show here. A clone is made
      when saving.
      <table class="table w-full table-zebra">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <UserRowCloneAtSaveWithoutFix v-for="user in users" :key="user.id" :user="user" />
        </tbody>
      </table>
    </div>
    <div v-if="users.length" class="my-3">
      [UserRowCloneAtSave.vue] <u>Fixed.</u> Data in this table loads items. An update from server will show here. A clone is made when saving.
      <br />
      A quick fix is to clear existing clone. This has the side effect of resetting the clone from first table, making it reactive to item again.
      <table class="table w-full table-zebra">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <UserRowCloneAtSave v-for="user in users" :key="user.id" :user="user" />
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
