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
        <div class="sticky">
          <Worldwide v-if="!activeCountry" />
          <Country
            v-else
            :activeCountry="activeCountry"
            @setActiveCountry="handleSetActiveCountry(null)"
          />
        </div>
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
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
:root {
  --color-pink: #ea5068;
  --color-dark-red: darken(#b93332, 10%);
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
    color: var(--color-pink);
  }
  .container {
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

    .stats {
      border-top: 30px;
      border-bottom: 30px;
      border-left: solid 1px #eaeaea;
      text-align: center;
      h2 {
        font-size: 20px;
      }
      h3 {
        font-size: 15px;
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
    margin: 10px 0;
    padding: 10px;
    text-align: center;

    .count {
      color: var(--color-pink);
      font-size: 1.5rem;
    }
    .date {
      color: var(--color-pink);
      font-size: 1.5rem;
    }
  }
}
</style>
