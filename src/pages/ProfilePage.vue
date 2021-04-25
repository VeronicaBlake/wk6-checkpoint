<template>
  <div class="ProfilePage"></div>
</template>

<script>
import { computed, onMounted, reactive } from 'vue'
import { AppState } from '../AppState'
import { profilesService } from '../services/ProfilesService'
import Notification from '../utils/Notification'
import { logger } from '../utils/Logger'
import { useRoute } from 'vue-router'
export default {
  name: 'Profile',
  setup() {
    const route = useRoute()
    const state = reactive({
      profile: computed(() => AppState.profile)
    })
    onMounted(async() => {
      await profilesService.getActiveProfile(route.params.id)
    })
    return {
      state,
      async create() {
        try {
          await profilesService.create(state.newProject)
          state.newProject = {}
          Notification.toast('Successfully Created Project', 'success')
        } catch (error) {
          logger.log(error)
          Notification.toast('Error: ' + error, 'error')
        }
      }
    }
  }
}
</script>

<style scoped>
img {
  max-width: 100px;
}
</style>
