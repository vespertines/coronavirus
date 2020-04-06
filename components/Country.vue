<template>
  <div class="country">
    <header>
      <h2>Cases in {{ activeCountry.countryRegion }}</h2>
    </header>
    <main>
      <div class="element">
        <h3>Confirmed Cases</h3>
        <div class="count">
          <span>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(
                activeCountry.confirmed
              )
            }}
          </span>
        </div>
      </div>
      <div class="element">
        <h3>Recovered</h3>
        <div class="count">
          <span>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(
                activeCountry.recovered
              )
            }}
          </span>
        </div>
      </div>
      <div class="element">
        <h3>Deaths</h3>
        <div class="count">
          <span>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(
                activeCountry.deaths
              )
            }}
          </span>
        </div>
      </div>
      <div class="element">
        <h3>Death Rate</h3>
        <div class="count">
          <span>{{ deathRate }}%</span>
        </div>
      </div>
      <div class="element">
        <span class="updated">
          Last updated on
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
    </main>
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
