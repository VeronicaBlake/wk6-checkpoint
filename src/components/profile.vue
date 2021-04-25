<template>
  <div class="row column-height card shadow m-2">
    <div class="col-12">
    </div>
  </div>
</template>

<script>
import { AuthService } from '../services/AuthService'
import { AppState } from '../AppState'
import { computed, reactive } from 'vue'
export default {
  name: 'Profile',
  props: {
    profile: {
      type: Object,
      required: true
    },
    setup() {
      const state = reactive
      return {
        state,
        user: computed(() => AppState.user),
        async login() {
          AuthService.loginWithPopup()
        },
        async logout() {
          await AuthService.logout({ returnTo: window.location.origin })
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.column-height {
  min-height: 100vh;
}
</style>
