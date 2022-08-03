<template>
  <tr>
    <th>{{ user.id }}</th>
    <td>
      <input v-model="user.name" type="text" class="input input-bordered" @input="() => save('name')" />
    </td>
    <td>
      <div class="inline-flex">
        <router-link :to="`/users/${user.id}`" class="btn btn-sm"> <feather-external-link class="mr-1" /> Edit in separate page </router-link>

        <button type="button" class="btn btn-error btn-sm" @click="() => user.remove()">Delete</button>
      </div>
    </td>
  </tr>
</template>

<script setup lang="ts">
import { debounce } from 'lodash'
import { useGet } from 'feathers-pinia'
import { useUserStore } from '~/stores/user'

const props = defineProps({
  userId: {
    type: Number,
    required: true
  }
})

const userStore = useUserStore()

// fetch item on userId change
watch(
  // the source we are watching
  () => props.userId,
  // the action when triggered
  () => useGet({ model: userStore.Model, id: props.userId })
)

const user = computed(() => {
  return userStore.getFromStore(props.userId)
})

const save_user = () => {
  user.value.save()
}

const save = debounce(save_user, 500)

// BAD : this does not work as user is a ComputedRef
/*
let { item: user } = useGet({ model: userStore.Model, id: props.userId })
watch(
  // the source we are watching
  () => props.userId,
  // the action when tirggered
  () => (user = useGet({ model: userStore.Model, id: props.userId }))
)
*/
</script>
