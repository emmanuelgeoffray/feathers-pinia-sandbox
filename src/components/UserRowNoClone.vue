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

const props = defineProps({
  user: {
    type: Object,
    required: true
  }
})
const { user } = toRefs(props)

const save_user = () => {
  user.value.save()
}

const save = debounce(save_user, 500)
</script>
