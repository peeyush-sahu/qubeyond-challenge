<template>
  <main style="padding: 20px">
    <a-breadcrumb style="margin-bottom: 20px">
      <a-breadcrumb-item><router-link to="/">Planets</router-link></a-breadcrumb-item>
      <a-breadcrumb-item>{{ planet?.name }}</a-breadcrumb-item>
    </a-breadcrumb>
    <a-spin :spinning="loading">
      <a-descriptions bordered :column="{ xxl: 4, xl: 3, lg: 3, md: 3, sm: 2, xs: 1 }">
        <a-descriptions-item label="Name">{{ planet?.name }}</a-descriptions-item>
        <a-descriptions-item label="Rotation Period">{{
          planet?.rotation_period
        }}</a-descriptions-item>
        <a-descriptions-item label="Diameter">{{ planet?.diameter }}</a-descriptions-item>
        <a-descriptions-item label="Climate">{{ planet?.climate }}</a-descriptions-item>
        <a-descriptions-item label="Orbital Period">{{
          planet?.orbital_period
        }}</a-descriptions-item>
        <a-descriptions-item label="Gravity">{{ planet?.gravity }}</a-descriptions-item>
        <a-descriptions-item label="Terrain">{{ planet?.terrain }}</a-descriptions-item>
        <a-descriptions-item label="Surface Water">{{ planet?.surface_water }}</a-descriptions-item>
        <a-descriptions-item label="Population">{{ planet?.population }}</a-descriptions-item>
        <a-descriptions-item label="Residents">
          <a-space direction="vertical">
            <a-typography-text v-for="resident in residents" :key="resident.name">{{
              resident.name
            }}</a-typography-text>
          </a-space>
        </a-descriptions-item>
        <a-descriptions-item label="Films">
          <a-space direction="vertical">
            <a-typography-text v-for="film in films" :key="film.title">{{
              film.title
            }}</a-typography-text>
          </a-space>
        </a-descriptions-item>
      </a-descriptions>
    </a-spin>
  </main>
</template>

<script>
export default {
  data() {
    return {
      loading: true,
      planet: {},
      residents: [],
      films: []
    }
  },

  created() {
    this.fetchData()
  },

  methods: {
    async fetchData() {
      const url = `https://swapi.dev/api/planets/${this.$route.params.id}?format=json`
      this.planet = await (await fetch(url)).json()
      this.residents = await Promise.all([
        ...this.planet.residents.map(async (residentUrl) => (await fetch(residentUrl)).json())
      ])
      this.films = await Promise.all([
        ...this.planet.films.map(async (filmUrl) => (await fetch(filmUrl)).json())
      ])
      this.loading = false
    }
  }
}
</script>
