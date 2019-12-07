<template>
  <div class="container">
    <Search/>
  </div>
</template>

<script>
// import Logo from '~/components/Logo.vue'
import Search from '~/components/Search.vue'
export default {
  components: {
    // Logo,
    Search
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  /* padding-top: 20px; */
  justify-content: center;
  /* align-items: center; */
  text-align: center;
}

</style>
