<template>
  <div class="list">
    <ul>
      <li>
        <span>Country</span>
        <span @click="sortMethod = 'confirmed'">
          Confirmed
          <br />
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
            <path
              fill="currentColor"
              d="M288 288H32c-28.4 0-42.8 34.5-22.6 54.6l128 128c12.5 12.5 32.8 12.5 45.3 0l128-128c20-20.1 5.7-54.6-22.7-54.6zM160 448L32 320h256L160 448zM32 224h256c28.4 0 42.8-34.5 22.6-54.6l-128-128c-12.5-12.5-32.8-12.5-45.3 0l-128 128C-10.7 189.5 3.6 224 32 224zM160 64l128 128H32L160 64z"
            />
          </svg>
        </span>
        <span @click="sortMethod = 'recovered'">
          Recovered
          <br />
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
            <path
              fill="currentColor"
              d="M288 288H32c-28.4 0-42.8 34.5-22.6 54.6l128 128c12.5 12.5 32.8 12.5 45.3 0l128-128c20-20.1 5.7-54.6-22.7-54.6zM160 448L32 320h256L160 448zM32 224h256c28.4 0 42.8-34.5 22.6-54.6l-128-128c-12.5-12.5-32.8-12.5-45.3 0l-128 128C-10.7 189.5 3.6 224 32 224zM160 64l128 128H32L160 64z"
            />
          </svg>
        </span>
        <span @click="sortMethod = 'deaths'">
          Deaths
          <br />
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
            <path
              fill="currentColor"
              d="M288 288H32c-28.4 0-42.8 34.5-22.6 54.6l128 128c12.5 12.5 32.8 12.5 45.3 0l128-128c20-20.1 5.7-54.6-22.7-54.6zM160 448L32 320h256L160 448zM32 224h256c28.4 0 42.8-34.5 22.6-54.6l-128-128c-12.5-12.5-32.8-12.5-45.3 0l-128 128C-10.7 189.5 3.6 224 32 224zM160 64l128 128H32L160 64z"
            />
          </svg>
        </span>
      </li>
      <li
        v-for="(country, index) in sortedCountries"
        :key="index"
        :class="{ active: selectedCountryIndex === country.countryRegion }"
      >
        <span @click="handleSelectedCountry(index)">
          {{ country.countryRegion }}
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
            <path
              fill="currentColor"
              d="M285.476 272.971L91.132 467.314c-9.373 9.373-24.569 9.373-33.941 0l-22.667-22.667c-9.357-9.357-9.375-24.522-.04-33.901L188.505 256 34.484 101.255c-9.335-9.379-9.317-24.544.04-33.901l22.667-22.667c9.373-9.373 24.569-9.373 33.941 0L285.475 239.03c9.373 9.372 9.373 24.568.001 33.941z"
            />
          </svg>
        </span>
        <span>{{ country.confirmed }}</span>
        <span>{{ country.recovered }}</span>
        <span>{{ country.deaths }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'List',
  props: {
    countries: Array,
    selectedCountryIndex: null
  },
  data() {
    return {
      sortMethod: 'confirmed'
    }
  },
  computed: {
    activeCountries() {
      if (!this.countries) return null
      return this.countries.reduce((countries, item) => {
        if (!countries[item.countryRegion]) {
          countries[item.countryRegion] = item
        } else {
          countries[item.countryRegion].confirmed += item.confirmed
          countries[item.countryRegion].recovered += item.recovered
          countries[item.countryRegion].deaths += item.deaths
        }
        return countries
      }, {})
    },
    sortedCountries() {
      return this.activeCountries
        ? Object.values(this.activeCountries).sort((a, b) => {
            return parseInt(b[this.sortMethod], 10) >
              parseInt(a[this.sortMethod], 10)
              ? 1
              : parseInt(b[this.sortMethod], 10) <
                parseInt(a[this.sortMethod], 10)
              ? -1
              : 0
          })
        : ''
    }
  },
  methods: {
    handleSelectedCountry(index) {
      this.$emit('setActiveCountry', this.sortedCountries[index])
    }
  }
}
</script>

<style lang="scss" scoped>
.list {
  height: 100vh;
  padding: 20px;
  overflow-y: scroll;
  @media (min-width: 1024px) {
    height: auto;
    min-height: 100vh;
    overflow-y: auto;
  }
  ul {
    list-style: none;
    margin: 0;
    padding: 0;
    li {
      cursor: pointer;
      font-size: 0.9em;
      padding: 0;
      display: grid;
      grid-template-columns: 2fr 1fr 1fr 1fr;
      padding: 3px 0;
      text-align: center;
      span:first-child {
        text-align: left;
      }
      span > svg {
        height: 18px;
        width: 18px;
        color: var(--color-red);
      }
      span:first-child > svg {
        color: inherit;
        height: 10px;
        width: 10px;
      }
      &:hover,
      &.active {
        color: var(--color-red);
        font-weight: 600;
      }
      &:first-child {
        font-size: 0.6em;
        font-weight: bold;
        padding: 7px 0;
        margin: 0px auto 10px auto;
        &:hover {
          color: inherit;
        }
      }
    }
  }
}
</style>
