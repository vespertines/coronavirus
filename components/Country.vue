<template>
  <div class="country">
    <header>
      <h2 class="title">Cases in {{ activeCountry.countryRegion }}</h2>
    </header>
    <div class="card-container">
      <div class="confirmed card">
        <h4>Confirmed Cases</h4>
        <span class="count">
          {{
          new Intl.NumberFormat({ style: 'unit' }).format(
          activeCountry.confirmed
          )
          }}
        </span>
      </div>
      <div class="recovered card">
        <h4>Recovered</h4>
        <span class="count">
          {{
          new Intl.NumberFormat({ style: 'unit' }).format(
          activeCountry.recovered
          )
          }}
        </span>
      </div>
      <div class="deaths card">
        <h4>Deaths</h4>
        <span class="count">
          {{
          new Intl.NumberFormat({ style: 'unit' }).format(activeCountry.deaths)
          }}
        </span>
      </div>
      <div class="death-rate card">
        <h4>Death Rate</h4>
        <span class="count">{{ deathRate }}%</span>
      </div>
      <div class="updated card">
        <h4>Last Updated</h4>
        <span class="date">
          {{
          new Date(activeCountry.lastUpdate).toLocaleDateString(undefined, {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric'
          })
          }}
        </span>
      </div>
    </div>
    <footer>
      <button @click="$emit('setActiveCountry')">View Global Stats</button>
    </footer>
  </div>
</template>

<script>
export default {
  props: {
    activeCountry: Object
  },
  computed: {
    deathRate() {
      const { confirmed, deaths } = this.activeCountry
      return ((deaths / confirmed) * 100).toFixed(2)
    }
  },
  data() {
    return {
      country: null
    }
  }
}
</script>

<style lang="scss" scoped>
.country {
  footer {
    text-align: center;
    button {
      background-color: var(--color-pink);
      border: none;
      color: #000;
      cursor: pointer;
      padding: 10px 30px;
      transition: background-color 0.2s ease-in-out;

      &:hover,
      &:focus {
        outline: none;
      }
    }
  }
}
</style>
