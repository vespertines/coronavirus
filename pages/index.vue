<template>
  <div>
    <div class="sidebar">
      <Sidebar />
    </div>
    <div class="contents">
      <Contents />
    </div>
  </div>
</template>

<script>
import Sidebar from '~/components/Sidebar.vue'
import Contents from '~/components/Contents.vue'

export default {
  components: {
    Sidebar,
    Contents
  }
}
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  min-height: 100%;
}

.sidebar {
  display: inline-block;
  border-right: solid 1px #eee;
  width: 330px;
  height: 100%;
}

.contents {
  display: inline-block;
  width: calc(100% - 335px);
}
</style>
