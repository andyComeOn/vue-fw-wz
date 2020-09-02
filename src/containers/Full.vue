<template>
  <div class="app">
    <AppHeader/>
    <div class="app-body">
      <Sidebar/>
      <main class="main">
        <!-- <breadcrumb :list="list"/> -->
        <div class="container-fluid">
          <router-view></router-view>
        </div>
      </main>
      <AppAside/>
    </div>
    <AppFooter/>
  </div>
</template>

<script>
import AppHeader from '../components/AppHeader'
import Sidebar from '../components/Sidebar'
import AppAside from '../components/AppAside'
import AppFooter from '../components/AppFooter'
import Breadcrumb from '../components/Breadcrumb'

export default {
  // name: 'full',
  components: {
    AppHeader,
    Sidebar,
    AppAside,
    AppFooter,
    Breadcrumb
  },
  computed: {
    name () {
      return this.$route.name
    },

    list () {
      return this.$route.matched
    }
  }
}
</script>
