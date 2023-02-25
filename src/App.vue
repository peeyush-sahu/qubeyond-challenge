<template>
  <a-page-header
    style="border: 1px solid rgb(235, 237, 240)"
    title="SWAPI"
    sub-title="The Star Wars API"
  />
  <RouterView />
</template>

<script setup>
import { RouterView } from 'vue-router'
</script>
