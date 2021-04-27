<template>
  <div class="container-fluid">
    <div class="row">
      <div class="card col-md-12 col-9 shadow flex-grow-1 post-card p-2 my-2">
        <div>
          <router-link :to="{name: 'Profile', params: {id:account.id}}">
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
          <p @click="like">
            likes: {{ post.likes.length }}
          </p>
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
      async like(id) {
        try {
          await postsService.likePost(props.post.id)
          Notification.toast('Deleted', 'sucess')
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
    height: 10rem
  }
</style>
