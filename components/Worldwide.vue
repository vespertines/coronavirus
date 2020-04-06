<template>
  <div class="worldwide">
    <header>
      <h2>Worldwide</h2>
    </header>
    <main>
      <section>
        <h3>Confirmed Cases</h3>
        <div class="count">
          <span>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(confirmedCases)
            }}
          </span>
        </div>
      </section>
      <section>
        <h3>Recovered</h3>
        <div class="count">
          <span>
            {{
              new Intl.NumberFormat({ style: 'unit' }).format(recoveredCases)
            }}
          </span>
        </div>
      </section>
      <section>
        <h3>Deaths</h3>
        <div class="count">
          <span>
            {{ new Intl.NumberFormat({ style: 'unit' }).format(totalDeaths) }}
          </span>
        </div>
      </section>
      <section>
        <h3>Death Rate</h3>
        <div class="count">
          <span>{{ deathRate }}%</span>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'Worldwide',
  data() {
    return {
      loading: true,
      confirmed: null,
      recovered: null,
      deaths: null
    }
  },
  computed: {
    confirmedCases() {
      return this.confirmed && this.confirmed.length
        ? this.confirmed.reduce((totalCases, country) => {
            totalCases += country.confirmed
            return totalCases
          }, 0)
        : null
    },
    recoveredCases() {
      return this.recovered && this.recovered.length
        ? this.recovered.reduce((totalCases, country) => {
            totalCases += country.recovered
            return totalCases
          }, 0)
        : null
    },
    totalDeaths() {
      return this.deaths && this.deaths.length
        ? this.deaths.reduce((totalDeaths, country) => {
            totalDeaths += country.deaths
            return totalDeaths
          }, 0)
        : null
    },
    deathRate() {
      return this.confirmedCases && this.totalDeaths
        ? ((this.totalDeaths / this.confirmedCases) * 100).toFixed(2)
        : 0
    }
  },
  async mounted() {
    try {
      let confirmed = await fetch('https://covid19.mathdro.id/api/confirmed')
      let recovered = await fetch('https://covid19.mathdro.id/api/recovered')
      let deaths = await fetch('https://covid19.mathdro.id/api/deaths')
      this.confirmed = await confirmed.json()
      this.recovered = await recovered.json()
      this.deaths = await deaths.json()
      this.loading = false
    } catch (err) {
      console.log(err)
    }
  }
}
</script>
