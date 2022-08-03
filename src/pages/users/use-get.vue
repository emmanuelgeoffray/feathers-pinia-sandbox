<template>
  <!--<div class="overflow-x-auto">-->
  <div>
    <button type="button" class="btn mb-4" @click="patchFirstUser">Mock patch from server</button>

    <div v-if="user" class="my-3">
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
          <UserRow :key="user.id" :user="user" />
        </tbody>
      </table>
    </div>
    <div v-if="user" class="my-3">
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
          <UserRowNoClone :key="user.id" :user="user" />
        </tbody>
      </table>
    </div>
    <div v-if="user" class="my-3">
      <u>Recommended :</u> [UserRowCloneAtSave.vue] Data in this table loads items. An update from server will show here. A clone is made when saving.
      <br />
      Note that there is an
      <a href="https://github.com/marshallswain/feathers-pinia/issues/52"> issue </a>
      with clones currently with feathers-pinia. A quick fix is to clear existing clone. This has the side effect of resetting the clone from first
      table, making it reactive to item again.
      <table class="table w-full table-zebra">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <UserRowCloneAtSave :key="user.id" :user="user" />
        </tbody>
      </table>
    </div>
    <div v-if="user">
      <OtherTableWithUseGet />
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
import { useGet } from 'feathers-pinia'
import { useUserStore } from '~/stores/user'
import { api } from '~/feathers'

const userStore = useUserStore()

// You can find User on the userStore's `Model` property OR...
const User = userStore.Model

// Since we called `useUserStore()` the model is also available on the `models` object.
// import { models, useFind } from 'feathers-pinia'
// const { User } = models.api

const { item: user } = useGet({ model: User, id: 0 })

function patchFirstUser() {
  api.service('users').patch('0', { name: 'Emmanuel' })
}
</script>
