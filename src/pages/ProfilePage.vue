<template>
  <div>
    <h1>Profile Page!</h1>
    {{ state.activeProfile }}
    <form @submit.prevent="createPost" v-if="user.isAuthenticated && user.userInfo.id === route.params.id">
      <input type="text" name="" id="">
    </form>
    <Post v-for="post in state.posts" :key="post.id" :post="post" />
  </div>
</template>

<script>
import { computed, onMounted, reactive } from 'vue'
import { AppState } from '../AppState'
import { profilesService } from '../services/ProfilesService'
import { postsService } from '../services/PostsService'
import Notification from '../utils/Notification'
import { logger } from '../utils/Logger'
import { useRoute } from 'vue-router'
import { accountService } from '../services/AccountService'
export default {
  name: 'Profile',
  setup() {
    const route = useRoute()
    const state = reactive({
      newPost: {},
      activeProfile: computed(() => AppState.activeProfile),
      user: computed(() => AppState.user),
      profile: computed(() => AppState.profile)
    })
    onMounted(async() => {
      await profilesService.getActiveProfile(route.params.id)
      await accountService.getProfilePosts(route.params.id)
    })
    return {
      state,
      async create() {
        try {
          await postsService.create(state.newProject)
          state.newProject = {}
          Notification.toast('Successfully Created', 'success')
          state.newPost = {}
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
