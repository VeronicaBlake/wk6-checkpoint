<template>
  <header>
    <Navbar />
  </header>
  <main class = "row">
    <div class = "col-md-9 col-12">
      <router-view />
    </div>
    <div class = "col-3 d-md-block d-none">
      <Markets />
    </div>
  </main>
  <footer>
    <div class="bg-primary text-light text-center p-4">
      Made with 💖 by Veronica
    </div>
  </footer>
</template>

<script>
import { computed } from 'vue'
import { AppState } from './AppState'
export default {
  name: 'App',
  setup() {
    return {
      appState: computed(() => AppState)
    }
  }
}
</script>
<style lang="scss">
@import "./assets/scss/main.scss";

</style>
