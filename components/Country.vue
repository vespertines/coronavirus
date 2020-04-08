<template>
  <div class="country">
    <header>
      <h2>{{ activeCountry.countryRegion }}</h2>
    </header>
    <main>
      <section>
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
      </section>
      <section>
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
      </section>
      <section>
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
      </section>
      <section>
        <h3>Death Rate</h3>
        <div class="count">
          <span>{{ deathRate }}%</span>
        </div>
      </section>
      <section>
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
      </section>
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
