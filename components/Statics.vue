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
      <section>
        <div class="sticky">
          <Global v-if="!activeCountry" />
          <Country
            v-else
            :activeCountry="activeCountry"
            @setActiveCountry="handleSetActiveCountry(null)"
          />
        </div>
      </section>
    </div>
    <div v-else class="container loading">
      Loading...
      <div class="spinner"></div>
    </div>
  </div>
</template>

<script>
import Country from './Country.vue'
import Global from './Global.vue'
import List from './List.vue'
export default {
  name: 'App',
  components: {
    Country,
    Global,
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
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
:root {
  --color-red: #b93332;
  --color-dark-red: darken(#b93332, 10%);
}
body,
html {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  padding: 60px 10px 0 10px;
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
      left: 300px;
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
  .highlight {
    color: var(--color-red);
  }
  .container {
    max-width: 1280px;
    position: relative;
    border-radius: 20px 20px 0 0;
    margin: auto;
    height: auto;
    min-height: 100vh;
    padding: 0;
    aside {
      position: fixed;
      top: 0;
      left: 0;
      width: 300px;
      transform: translateX(-300px);
      transition: transform 0.3s ease-in-out;
      z-index: 3;
      &.open {
        transform: translateX(0);
      }
    }
    h1 {
      text-align: center;
      margin: 0;
    }
    .title {
      font-size: 2.6em;
      margin-top: 0;
      text-align: center;
    }
    section {
      padding: 20px;
      .sticky {
        position: sticky;
        top: 40px;
      }
      .og-image {
        img {
          width: 100%;
        }
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
      grid-template-columns: 400px 1fr;
      aside {
        position: static;
        width: auto;
        transform: translateX(0px);
        height: auto;
        .list {
          overflow: auto;
        }
      }
    }
  }
  .card-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    grid-gap: 20px;
    margin-bottom: 30px;
    > .card {
      margin: 0;
    }
    @media (min-width: 1024px) {
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      .card {
        &.death-rate,
        &.updated {
          grid-column: 1 / -1;
        }
      }
    }
    @media (min-width: 1366px) {
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      .card {
        &.death-rate,
        &.updated {
          grid-column: 1 / -1;
        }
      }
    }
  }
  .card {
    border-radius: 10px;
    margin: 30px 0;
    padding: 20px;
    text-align: center;
    h4 {
      margin: 0 0 30px 0;
    }
    .count {
      color: var(--color-red);
      font-size: 3rem;
    }
    .date {
      color: var(--color-red);
      font-size: 1.4rem;
    }
  }
}
</style>
