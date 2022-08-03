<template>
  <!--<div class="overflow-x-auto">-->
  <div>
    <h1 class="title">Hello {{ users[0]?.name }} !</h1>
    <div>This page showcases the use of different patterns in feathers-pinia.</div>
    <button type="button" class="btn mt-4 mb-2" @click="patchFirstUser">Mock patch from server</button>

    <div v-if="users.length" class="my-3">
      [UserRow.vue] This component uses the
      <a class="link" href="https://feathers-pinia.pages.dev/guide/handle-clones.html">Handle clones</a>
      pattern.
      <br />
      Cons: A clone is not reactive with server data. Hit button below above to notice no changes.
      <br />
      Pros: Makes working with form data easier and respect Vue's rules
      <ul>
        <li>
          <a class="link" href="https://vuejs.org/style-guide/rules-use-with-caution.html#implicit-parent-child-communication"
            >1. implicit-parent-child-communication</a
          >
        </li>
        <li>
          <a class="link" href="https://vuex.vuejs.org/guide/strict.html">2. Strict mode (do not mutate state outside of mutation handlers)</a>
        </li>
      </ul>
      Use case: A user is editing a form that you want to be 'locked' while user is editing. Or a user is making changes to data and you do not want
      to update visible data until you commit or save the data. Do not use for data that needs reactivity, e.g. a todo list synced between two
      sessions.
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
      [UserRowNoClone.vue] This component modifies directly the user object passed as a prop. The input is reactive with a two-way binding : a change
      from server is shown in input, a change in input is saved to server.
      <br />
      To confirm that behavior, open Vue DevTools, > Inspector > Pinia, service.users, state.itemsById.0.name and notice how a change in input is seen
      in the pinia store.
      <br />
      Pros: Makes working with reactive data easier. The user object in store is a
      <a href="https://vuejs.org/guide/essentials/reactivity-fundamentals.html">reactive</a>. That's a new thing in Vue 3 that you can modify an
      object property and maintain reactivity. <br />
      Notice how a change to `user.name` is instantly visible on other inputs, even before saving in database.
      <br />
      Cons: This breaks Vue's rule named
      <a class="link" href="https://vuejs.org/style-guide/rules-use-with-caution.html#implicit-parent-child-communication"
        >implicit-parent-child-communication</a
      >. The child component modifies directly the object passed as a prop while an ideal Vue application is props down, events up.
      <br />
      Use case: A user is editing an object that needs to be reactive with changes in others components in the same page, or changes from server, e.g.
      a todo list synced between two sessions.

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
    <div v-if="users.length" class="my-3">
      [UserRowCloneAtSave.vue] This component initiates a clone on input change, modifies the clone with the new value, and commit. A clone
      <a class="link" href="https://feathers-pinia.pages.dev/guide/model-instances.html#instance-clone-params">commit</a> triggers a mutation to sync
      with store's item.
      <br />
      Note that there is an
      <a class="link" href="https://github.com/marshallswain/feathers-pinia/issues/52"> issue </a>
      with clones currently with feathers-pinia. A quick fix is to clear existing clone.
      <br />
      Pros: Reactivity works and this pattern respects the two Vue rules on
      <a class="link" href="https://vuejs.org/style-guide/rules-use-with-caution.html#implicit-parent-child-communication"
        >implicit-parent-child-communication</a
      >
      and
      <a class="link" href="https://vuex.vuejs.org/guide/strict.html">Strict mode (do not mutate state outside of mutation handlers)</a>
      <br />
      Cons: Less convenient to use. You need to define a computed with a setter for each object's property, and write extra code to clone and commit
      before saving.
      <br />
      Use case: A user is editing an object that needs to be reactive with changes in others components in the same page, or changes from server, e.g.
      a todo list synced between two sessions.

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
    <div v-if="users.length" class="my-3">
      [UserRowById.vue] This components receives the userId as a prop and fetches the user from store with
      <a class="link" href="https://feathers-pinia.pages.dev/guide/use-get.html">useGet</a>
      <div>
        Pros: Reactivity works and this pattern respects the two Vue rules on
        <a class="link" href="https://vuejs.org/style-guide/rules-use-with-caution.html#implicit-parent-child-communication"
          >implicit-parent-child-communication</a
        >
        and
        <a class="link" href="https://vuex.vuejs.org/guide/strict.html">Strict mode (do not mutate state outside of mutation handlers)</a>
      </div>
      <div>
        Cons: If the userId prop is changed, component is not refreshed with content from new user. Hit button below to add a user and notice how the
        input does not change.
      </div>

      <div>
        <button type="button" class="btn mt-4 mb-2" @click="addUser">Add user</button>
      </div>

      <table class="table w-full table-zebra">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <UserRowById :userId="lastUser.id" />
        </tbody>
      </table>
    </div>

    <div v-if="users.length" class="my-3">
      <u>Recommended :</u>
      [UserRowById.vue] with a
      <a class="link" href="" target="_blank">`key` special attribute</a>.
      <div>
        This components receives the userId as a prop and fetches the user from store with
        <a class="link" href="https://feathers-pinia.pages.dev/guide/use-get.html">useGet</a>
      </div>
      <div>
        Pros: Reactivity works and a change in userId changes the input. The `key` special attributes forces replacement of the component instead of
        reusing it.
      </div>
      <div>
        Cons: The component is replaced instead of patched, this can have a CPU cost depending on the complexity of the component, and triggers
        transition animations.
      </div>
      <div>
        Use case: A quick edit component docked on a page that is reused to edit different items. The component is reactive with data changed in other
        component or server update.
      </div>

      <table class="table w-full table-zebra">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <UserRowById :key="lastUser.id" :userId="lastUser.id" />
        </tbody>
      </table>
    </div>
    <div v-if="users.length" class="my-3">
      [UserRowByIdAndReactive.vue]
      <div>
        This components receives the userId as a prop. It uses `watch` to track changes on userId and triggers an action to fetch user from store. The
        `user` object is a `computed` to reflect changes of userId. It may look counter-intuitive to make changes to `user.name` in the template as
        user is a computed. This works because user is an object.
      </div>
      <div>Pros: Reactivity works and a change in userId changes the input. The component is not replaced, it's patched with new values.</div>
      <div>
        Cons: This breaks Vue's rule named
        <a class="link" href="https://vuejs.org/guide/essentials/computed.html#writable-computed"> Avoid mutating computed value </a>
        and mutating a computed that has no setter makes your code hard to understand.
      </div>
      <div>
        Use case: A quick edit component docked on a page that is reused to edit different items. The component is reactive with data changed in other
        component or server update. You prefer to patch the component rather than replacing it.
      </div>

      <table class="table w-full table-zebra">
        <thead>
          <tr>
            <th></th>
            <th>Name</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <UserRowByIdAndReactive :userId="lastUser.id" />
        </tbody>
      </table>
    </div>

    <div v-if="users.length">
      <OtherTable />
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

const lastUser = computed(() => users.value[users.value.length - 1])

function patchFirstUser() {
  api.service('users').patch('0', { name: 'Emmanuel' })
}

function addUser() {
  api.service('users').create({ id: 1, name: 'Martin' })
}
</script>
