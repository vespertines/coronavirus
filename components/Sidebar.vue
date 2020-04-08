<template>
  <div class="sidebar">
    <div class="logo">
      <a href="/">
        <img src="@/static/images/logo.svg" />
      </a>
    </div>
    <div class="credit">
      <span>
        Built by
        <a href="mailto:atelier.jullian@gmail.com">Kwang-Su Kim</a>
      </span>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.sidebar {
  padding-top: 690px;
  text-align: center;
  @media (min-width: 1024px) {
    position: fixed;
    top: 0;
    left: 0;
    padding-top: 0;
    width: 270px;
    height: 100vh;
  }
  .logo {
    padding-top: 10px;
    padding-bottom: 10px;
    @media (min-width: 1024px) {
      padding-top: 30px;
      padding-bottom: 30px;
    }
    img {
      max-width: 100%;
      width: 120px;
      @media (min-width: 1024px) {
        width: auto;
      }
    }
  }
  .credit {
    text-align: center;
    @media (min-width: 1024px) {
      position: fixed;
      bottom: 30px;
      left: 0;
      width: 100%;
      width: 270px;
    }
    span {
      color: var(--color-grey);
      font-size: 10px;
      font-weight: 500;
      a {
        color: var(--color-grey);
        text-decoration: none;
      }
    }
  }
}
</style>
