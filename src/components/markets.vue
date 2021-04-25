<template>
  <div class="row w-100 column-height mt-3">
    <div class="col-12" v-if="markets[0] != null">
      <img class="img-fluid mb-3" :src="markets[0].tall" alt="">
      <img class="img-fluid my-3" :src="markets[1].tall" alt="">
    </div>
  </div>
</template>

<script>
import { marketsService } from '../services/MarketsService'
import { computed, onMounted } from 'vue'
import { AppState } from '../AppState'
import Notification from '../utils/Notification'
export default {
  name: 'Markets',
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  setup() {
    onMounted(async() => {
      try {
        await marketsService.getAllMarkets()
      } catch (error) {
        Notification.toast('Error: ' + error, 'error')
      }
    })
    return {
      markets: computed(() => AppState.markets)
    }
  },
  components: {}
}
</script>

<style lang="scss" scoped>
.column-height {
  min-height: 100vh;
}
</style>
