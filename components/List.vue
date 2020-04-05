<template>
  <div class="list">
    <table>
      <thead>
        <tr>
          <th>Location</th>
          <th @click="sortㅅMethod = 'confirmed'">
            Confirmed
            <!-- <br />
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
              <path
                fill="currentColor"
                d="M288 288H32c-28.4 0-42.8 34.5-22.6 54.6l128 128c12.5 12.5 32.8 12.5 45.3 0l128-128c20-20.1 5.7-54.6-22.7-54.6zM160 448L32 320h256L160 448zM32 224h256c28.4 0 42.8-34.5 22.6-54.6l-128-128c-12.5-12.5-32.8-12.5-45.3 0l-128 128C-10.7 189.5 3.6 224 32 224zM160 64l128 128H32L160 64z"
              />
            </svg>-->
          </th>
          <th @click="sortMethod = 'recovered'">
            Recovered
            <!-- <br />
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
              <path
                fill="currentColor"
                d="M288 288H32c-28.4 0-42.8 34.5-22.6 54.6l128 128c12.5 12.5 32.8 12.5 45.3 0l128-128c20-20.1 5.7-54.6-22.7-54.6zM160 448L32 320h256L160 448zM32 224h256c28.4 0 42.8-34.5 22.6-54.6l-128-128c-12.5-12.5-32.8-12.5-45.3 0l-128 128C-10.7 189.5 3.6 224 32 224zM160 64l128 128H32L160 64z"
              />
            </svg>-->
          </th>
          <th @click="sortMethod = 'deaths'">
            Deaths
            <!-- <br />
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
              <path
                fill="currentColor"
                d="M288 288H32c-28.4 0-42.8 34.5-22.6 54.6l128 128c12.5 12.5 32.8 12.5 45.3 0l128-128c20-20.1 5.7-54.6-22.7-54.6zM160 448L32 320h256L160 448zM32 224h256c28.4 0 42.8-34.5 22.6-54.6l-128-128c-12.5-12.5-32.8-12.5-45.3 0l-128 128C-10.7 189.5 3.6 224 32 224zM160 64l128 128H32L160 64z"
              />
            </svg>-->
          </th>
        </tr>
      </thead>

      <tbody
        v-for="(country, index) in sortedCountries"
        :key="index"
        :class="{ active: selectedCountryIndex === country.countryRegion }"
      >
        <tr>
          <td @click="handleSelectedCountry(index)">{{ country.countryRegion }}</td>
          <td>{{ country.confirmed }}</td>
          <td>{{ country.recovered }}</td>
          <td>{{ country.deaths }}</td>
        </tr>
      </tbody>
    </table>
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
  width: 100%;
  height: 100vh;

  // @media (min-width: 1024px) {
  //   height: auto;
  //   min-height: 100vh;
  //   overflow-y: auto;
  // }

  table {
    border-top: none;
    border-bottom: none;
    width: 100%;
    list-style: none;
    margin: 0;
    text-align: left;
    position: relative;
    border-collapse: collapse;

    tr {
      display: grid;
      grid-template-columns: 2fr 1fr 1fr 1fr;
      padding-top: 5px;
      padding-bottom: 5px;
      cursor: pointer;

      th {
        position: -webkit-sticky;
        position: sticky;
        top: 0;
        z-index: 2;
      }

      th[scope='row'] {
        position: -webkit-sticky;
        position: sticky;
        left: 0;
        z-index: 1;
      }

      th,
      td {
        padding-left: 30px;
        padding-right: 30px;
      }

      td > svg {
        height: 18px;
        width: 18px;
        color: var(--color-pink);
      }
      td:first-child > svg {
        color: inherit;
        height: 10px;
        width: 10px;
      }
      &:hover,
      &.active {
        color: var(--color-pink);
        font-weight: 600;
      }
      &:first-child {
        font-size: 15px;
        font-weight: bold;
        margin: 0px auto 10px auto;
        &:hover {
          color: inherit;
        }
      }
    }
  }
}
</style>
