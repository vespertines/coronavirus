<template>
  <div class="worldwide">
    <h2>Worldwide</h2>
    <div class="card-container">
      <div class="confirmed card">
        <h4>Confirmed Cases</h4>
        <span class="count">
          {{
          new Intl.NumberFormat({ style: 'unit' }).format(confirmedCases)
          }}
        </span>
      </div>
      <div class="recovered card">
        <h4>Recovered</h4>
        <span class="count">
          {{
          new Intl.NumberFormat({ style: 'unit' }).format(recoveredCases)
          }}
        </span>
      </div>
      <div class="deaths card">
        <h4>Deaths</h4>
        <span class="count">
          {{
          new Intl.NumberFormat({ style: 'unit' }).format(totalDeaths)
          }}
        </span>
      </div>
    </div>
    <div class="card">
      <h4>Death Rate</h4>
      <span class="count">{{ deathRate }}%</span>
    </div>
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
