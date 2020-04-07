<template>
  <div id="app">
    <div @click="navOpen = !navOpen" :class="{ open: navOpen }" class="close">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        width="20px"
        height="20px"
        viewBox="0 0 20 20"
        version="1.1"
      >
        <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
          <g id="close" fill="#000000" fill-rule="nonzero">
            <polygon
              id="Path"
              points="20 2.35675659 17.6432434 0 9.99997414 7.64321755 2.35675659 0 -5.68434189e-14 2.35675659 7.64321755 9.99997414 -5.64758971e-14 17.6432434 2.35675659 20 9.99997414 12.3567825 17.6431917 20 19.9999483 17.6432434 12.3567825 9.99997414"
            />
          </g>
        </g>
      </svg>
    </div>
    <div class="content" v-if="!loading">
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
    <div v-else class="content loading">
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
  left: 0;
  width: 100%;
  @media (min-width: 1024px) {
    left: 330px;
    width: calc(100% - 330px);
  }
  .close {
    position: fixed;
    top: 15px;
    right: 15px;
    z-index: 10;
    transition: right 1s ease-in-out;
    @media (min-width: 1024px) {
      display: none;
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
  .content {
    margin: 0;
    padding: 0;
    min-height: 100vh;
    background-color: var(--color-lighter-grey);
    aside {
      position: fixed;
      top: 0;
      left: 0;
      z-index: 3;
      transform: translateX(-100%);
      transition: transform 0.5s ease-in-out;
      width: 100%;
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
      border-left: solid 1px var(--color-light-grey);
      .spinner {
        border: 2px solid var(--color-pink);
        border-bottom-color: transparent;
        border-radius: 50%;
        height: 30px;
        width: 30px;
        animation: spin 0.5s linear infinite;
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
    padding-bottom: 360px;
    text-align: center;
    @media (min-width: 1024px) {
      padding-bottom: 30px;
    }
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
      @media (min-width: 1024px) {
        position: fixed;
        bottom: 0;
        right: 0;
        width: 330px;
      }
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
