<template>
  <div class="container-fluid">
    <div class="row">
      <div class="card col-md-11 col-9 shadow flex-grow-1 post-card p-2 my-2">
        <h5 class="text-left mx-4 " v-if="post.creator">
          <span><img class="rounded-circle img-icon" :src="post.creator.picture" alt=""></span> {{ post.creator.name }}
        </h5>
        <div>
          <h6 class="body-text">
            <span>{{ post.body }}</span>
          </h6>
          <img class="card-img-bottom w-100" :src="post.imgUrl" alt="">
        </div>
        <!-- Link to Individual Blog when clicked -->
        <!-- <router-link :to="{name:'HomePage', params: {id: post.id}}"> router linksa re for changing pages, link this to the creator profile-->
        <!-- </router-link> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Post',
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  setup() {
    return {}
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
