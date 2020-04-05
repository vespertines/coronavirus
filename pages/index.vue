<template>
  <div>
    <Sidebar />
    <Statics />
  </div>
</template>

<script>
import Sidebar from '~/components/Sidebar.vue'
import Statics from '~/components/Statics.vue'

export default {
  components: {
    Sidebar,
    Statics
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@500&display=swap');

html,
body {
  margin: 0;
  padding: 0;
  min-height: 100%;
  box-sizing: border-box;
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
