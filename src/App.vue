<template>
  <div
    id="app"
    v-page-loading="app.loading"
  >
    <header-component />
    <div class="app-container">
      <sidebar />
      <config-panel />
      <preview />
    </div>
  </div>
</template>

<script>
import Sidebar from './components/Sidebar'
import ConfigPanel from './components/ConfigPanel'
import Preview from './components/Preview'
import HeaderComponent from './components/Header'
import { mapState } from 'vuex'

export default {
  name: 'App',
  components: {
    Sidebar,
    ConfigPanel,
    Preview,
    HeaderComponent
  },

  computed: {
    ...mapState(['app', 'basic', 'options'])
  },

  async created () {
    this.$store.commit('SET_LOADING', true)
    await this.$store.dispatch('addInitialProject')
    this.$store.commit('SET_LOADING', false)
  }
}
</script>

<style lang="scss">
html,
body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 14px;
}
#app {
  display: grid;
  grid-template-rows: auto 1fr;
  height: 100vh;
}
.app-container {
  display: grid;
  grid-template-columns: 85px 550px 1fr;
  height: 100%;
}
.desc {
  flex-grow: 1;
  font-size: 12px;
  line-height: 1.5em;
  color: #aaa;
}
</style>
