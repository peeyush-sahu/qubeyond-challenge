<template>
  <main style="padding: 20px">
    <a-breadcrumb style="margin-bottom: 20px">
      <a-breadcrumb-item><router-link to="/">Planets</router-link></a-breadcrumb-item>
    </a-breadcrumb>
    <a-list
      :grid="{ gutter: 16, xs: 1, sm: 2, md: 2, lg: 3, xl: 3 }"
      :data-source="planets"
      :loading="initLoading || loading"
    >
      <template #loadMore>
        <div
          v-if="!initLoading && !loading && nextPage"
          :style="{ textAlign: 'center', marginTop: '12px', height: '32px', lineHeight: '32px' }"
        >
          <a-button @click="loadMore">Show More</a-button>
        </div>
      </template>
      <template #renderItem="{ item, index }">
        <a-list-item>
          <a-card style="border-radius: 6px" :title="item.name">
            <template #extra
              ><router-link :to="'/' + Number(index + 1)">Show Details</router-link></template
            >
            <a-card-meta>
              <template #description>
                <a-space direction="vertical">
                  <a-typography-text
                    >Rotation Period - {{ item.rotation_period }}</a-typography-text
                  >
                  <a-typography-text>Oribtal Period - {{ item.orbital_period }}</a-typography-text>
                  <a-typography-text>Population - {{ item.population }}</a-typography-text>
                </a-space>
              </template>
            </a-card-meta>
          </a-card>
        </a-list-item>
      </template>
    </a-list>
  </main>
</template>

<script>
export default {
  data() {
    return {
      initLoading: true,
      loading: false,
      planets: [],
      nextPage: null
    }
  },

  created() {
    this.fetchData()
  },

  methods: {
    async fetchData() {
      const url = `https://swapi.dev/api/planets/?format=json`
      const data = await (await fetch(url)).json()
      this.planets = data?.results
      this.nextPage = data?.next
      this.initLoading = false
    },

    async loadMore() {
      this.loading = true
      const data = await (await fetch(this.nextPage)).json()
      this.planets = this.planets.concat(data?.results)
      this.nextPage = data?.next
      this.loading = false
    }
  }
}
</script>
