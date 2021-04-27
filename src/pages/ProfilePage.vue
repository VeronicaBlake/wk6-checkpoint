<template>
  <div>
    <h1>Profile Page!</h1>
    {{ state.activeProfile }}
    <form @submit.prevent="createPost" v-if="state.user.isAuthenticated && state.account.id === route.params.id">
      <input type="text" name="" id="">
    </form>
  </div>
  <div>
    <Post v-for="post in activePosts" :key="post.id" :post="post" />
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
  name: 'ProfilePage',
  setup() {
    const route = useRoute()
    const state = reactive({
      newPost: {},
      activeProfile: computed(() => AppState.activeProfile),
      user: computed(() => AppState.user),
      account: computed(() => AppState.account)
    })
    onMounted(async() => {
      try {
        await profilesService.getActiveProfile(route.params.id)
        await profilesService.getProfilePosts(route.params.id)
        await accountService.getProfile(route.params.id)
      } catch (error) {
        Notification.toast(error, 'error')
      }
    })
    return {
      state,
      route,
      activePosts: computed(() => AppState.activePosts),
      async createPost() {
        try {
          await postsService.createPost(state.newPost)
          Notification.toast('Successfully Created', 'success')
          state.newPost = {}
        } catch (error) {
          logger.log(error)
          Notification.toast(error, 'error')
        }
      }
    }
  },
  components: {}
}
</script>

<style scoped>
img {
  max-width: 100px;
}
</style>
