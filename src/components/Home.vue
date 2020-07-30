<template>
  <div class="home">
    <cool-nav>导航</cool-nav>
  </div>
</template>

<script>
  import coolNav from './coolNav/coolNav'
export default {
  name: 'Home',
  components: {
    'cool-nav':coolNav
  },
  methods:{

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
