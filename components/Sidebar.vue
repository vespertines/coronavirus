<template>
  <div class="sidebar">
    <div class="logo">
      <nuxt-link to="/">
        <img src="@/static/images/logo.svg" />
      </nuxt-link>
    </div>
  </div>
</template>

<style lang="scss">
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: 330px;
  height: 100vh;
  border-right: solid 1px #eaeaea;
  text-align: center;

  .logo {
    padding-top: 30px;
    padding-bottom: 30px;
  }
}
</style>
