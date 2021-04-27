<template>
  <div v-if="post != null">
    <div class="row bg-light my-2 shadow">
      <div class="card col-md-12 col-9 flex-grow-1 post-card p-2">
        <div>
          <router-link :to="{name: 'ProfilePage', params: {id:post.creator.id}}">
            <img class="rounded-circle img-icon" :src="post.creator.picture" alt=""> {{ post.creator.name }}
          </router-link>
          <h5 class="text-left mx-4 " v-if="post.creator">
            <div> Created at: {{ post.createdAt }}</div>
          </h5>
          <button v-if="state.account && state.account.id == post.creatorId" class="btn btn-danger" @click="deletePost">
            Delete
          </button>
        </div>
        <h6 class="body-text">
          <span>{{ post.body }}</span>
          <img class="card-img-bottom w-100" v-if="post.imgUrl" :src="post.imgUrl" alt="post.body">
        </h6>
        <div>
          <h3>
            <i class="fas fa-heart text-danger" @click="like"></i> {{ post.likes.length }}
          </h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, reactive } from 'vue'
import { AppState } from '../AppState'
import { postsService } from '../services/PostsService'
import Notification from '../utils/Notification'
export default {
  name: 'Post',
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const state = reactive({
      account: computed(() => AppState.account)
    })
    return {
      state,
      async deletePost() {
        try {
          await postsService.deletePost(props.post.id)
          Notification.toast('Deleted', 'sucess')
        } catch (error) {
          Notification.toast(error, 'error')
        }
      },
      async like() {
        try {
          await postsService.likePost(props.post.id)
        } catch (error) {
          Notification.toast(error, 'error')
        }
      }

    }
  }
}
</script>

<style lang="scss" scoped>
  .img-icon {
    width: 4rem;
  }

  .body-text{
      color: darkslategrey
  }

  .post-card {
    height: 13rem
  }
</style>
