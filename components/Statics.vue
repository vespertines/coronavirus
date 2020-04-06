<template>
  <div id="app">
    <div @click="navOpen = !navOpen" :class="{ open: navOpen }" class="burger">
      <span></span>
      <span></span>
      <span></span>
    </div>
    <div class="container" v-if="!loading">
      <aside :class="{ open: navOpen }">
        <List
          :countries="countries"
          :selectedCountryIndex="activeCountryRegion"
          @setActiveCountry="handleSetActiveCountry"
        />
      </aside>
      <div class="stats">
        <Worldwide v-if="!activeCountry" />
        <Country
          v-else
          :activeCountry="activeCountry"
          @setActiveCountry="handleSetActiveCountry(null)"
        />
      </div>
    </div>
    <div v-else class="container loading">
      Loading...
      <div class="spinner"></div>
    </div>
  </div>
</template>

<script>
import Country from './Country.vue'
import Worldwide from './Worldwide.vue'
import List from './List.vue'
export default {
  name: 'App',
  components: {
    Country,
    Worldwide,
    List
  },
  watch: {
    activeCountryRegion() {
      this.navOpen = false
      window.scrollTo(0, 0)
    }
  },
  computed: {
    activeCountryRegion() {
      return this.activeCountry ? this.activeCountry.countryRegion : ''
    }
  },
  data() {
    return {
      countries: null,
      activeCountry: null,
      loading: true,
      navOpen: false
    }
  },
  async mounted() {
    try {
      let data = await fetch('https://covid19.mathdro.id/api/confirmed')
      let json = await data.json()
      this.countries = json.filter(country => country.confirmed)
      this.loading = false
    } catch (err) {
      console.log(err)
    }
  },
  methods: {
    handleSetActiveCountry(country) {
      this.activeCountry = country
    }
  }
}
</script>

<style lang="scss">
:root {
  --color-pink: #ea5068;
  --color-black: #000000;
  --color-white: #ffffff;
  --color-grey: #999999;
  --color-light-grey: rgba(0, 0, 0, 0.1);
  --color-lighter-grey: rgba(0, 0, 0, 0.05);
}
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
#app {
  position: absolute;
  top: 0;
  left: 330px;
  width: calc(100% - 330px);
  .burger {
    position: fixed;
    top: 10px;
    left: 10px;
    z-index: 4;
    background-color: #000;
    padding: 0 10px;
    transition: left 0.3s ease-in-out;
    @media (min-width: 1024px) {
      display: none;
    }
    span {
      display: block;
      width: 33px;
      height: 2px;
      background-color: #fff;
      margin: 10px auto;
      transition: transform 0.3s;
    }
    &.open {
      left: 100%;
      padding-right: 0;
      span {
        transform-origin: left;
        &:first-child {
          transform: rotate(45deg);
        }
        &:nth-child(2) {
          opacity: 0;
        }
        &:last-child {
          transform: rotate(-45deg);
        }
      }
    }
  }
  .container {
    margin: 0;
    padding: 0;
    min-height: 100vh;
    aside {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      transform: translateX(-100%);
      transition: transform 0.5s ease-in-out;
      z-index: 3;
      &.open {
        transform: translateX(0);
      }
    }
    &.loading {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      .spinner {
        border: 2px solid #fff;
        border-bottom-color: transparent;
        border-radius: 50%;
        height: 50px;
        width: 50px;
        margin-top: -100px;
        animation: spin 0.4s linear infinite;
      }
    }
    @media (min-width: 1024px) {
      display: grid;
      grid-template-columns: 1fr 330px;
      aside {
        position: static;
        width: auto;
        transform: translateX(0px);
        height: auto;
      }
    }
  }

  .stats {
    padding-top: 30px;
    padding-bottom: 30px;
    text-align: center;
    h2 {
      margin-top: 30px;
      margin-bottom: 60px;
      color: var(--color-black);
      font-size: 25px;
      font-weight: 700;
      letter-spacing: -0.025em;
    }
    h3 {
      margin-top: 0;
      margin-bottom: 10px;
      color: var(--color-grey);
      font-size: 15px;
      font-weight: 500;
    }
    .count {
      padding-bottom: 30px;
      span {
        color: var(--color-pink);
        font-size: 30px;
        font-weight: 700;
        letter-spacing: -0.025em;
      }
    }
    .updated {
      color: var(--color-grey);
      font-size: 10px;
      font-weight: 500;
    }
    footer {
      position: fixed;
      bottom: 0px;
      right: 0;
      width: 330px;
      button {
        margin-top: 30px;
        margin-bottom: 30px;
        padding: 10px 30px;
        border: solid 1px var(--color-light-grey);
        transition: background-color 0.5s ease-in-out;
        cursor: pointer;
        color: var(--color-grey);
        font-size: 10px;
        font-weight: 600;
        &:hover,
        &:focus {
          outline: none;
          background-color: var(--color-lighter-grey);
        }
      }
    }
  }
}
</style>
