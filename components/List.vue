<template>
  <div class="list">
    <table>
      <thead>
        <tr>
          <th>Location</th>
          <th @click="sortMethod = 'confirmed'">
            Confirmed
          </th>
          <th @click="sortMethod = 'recovered'">
            Recovered
          </th>
          <th @click="sortMethod = 'deaths'">
            Deaths
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(country, index) in sortedCountries"
          :key="index"
          :class="{ active: selectedCountryIndex === country.countryRegion }"
          @click="handleSelectedCountry(index)"
        >
          <td>
            {{ country.countryRegion }}
          </td>
          <td>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(country.confirmed)
            }}
          </td>
          <td>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(country.recovered)
            }}
          </td>
          <td>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(country.deaths)
            }}
          </td>
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
  overflow: auto;
  border-left: solid 1px var(--color-light-grey);
  border-right: solid 1px var(--color-light-grey);
  table {
    margin: 0;
    background-color: var(--color-white);
    border-top: none;
    border-bottom: none;
    border-collapse: collapse;
    width: 100%;
    text-align: left;
    thead {
      tr {
        position: sticky;
        top: 0;
        border-bottom: solid 1px var(--color-light-grey);
        th {
          padding-top: 20px;
          padding-bottom: 20px;
          color: var(--color-grey);
          font-size: 15px;
          font-weight: 500;
        }
      }
    }
    tbody {
      tr {
        border-bottom: solid 1px var(--color-lighter-grey);
        td {
          padding-top: 10px;
          padding-bottom: 10px;
          color: var(--color-black);
          &:first-child {
            font-weight: 700;
          }
        }
        &:hover,
        &.active {
          td {
            color: var(--color-pink);
          }
        }
      }
    }
    tr {
      display: grid;
      grid-template-columns: 3fr 1fr 1fr 1fr;
      cursor: pointer;
      th,
      td {
        padding-left: 30px;
        padding-right: 30px;
        background-color: var(--color-white);
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
    }
  }
}
</style>
