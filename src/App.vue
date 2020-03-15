<template>
  <div id="app">
    <News />
  </div>
</template>

<script>
import News from './components/News.vue'
import './assets/sass/main.sass'

export default {
  name: 'App',
  components: {
    News,
  }
}
</script>