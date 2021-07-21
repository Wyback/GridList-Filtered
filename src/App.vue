<template>
    <div class="filters">
      <Filters />
    </div>
</template>

<script>
import Filters from './components/Filters.vue'

export default {
  name: 'App',
  components: {
    Filters
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container{
  display: flex;
  height: 100px;
}

@media screen and (max-width: 2560px){
    .filters{
      width: 21%;
    }
  }

  @media screen and (max-width: 2000px){
    .filters{
      width: 30%;
    }
  }

@media screen and (max-width: 1440px){
    .filters{
      width: 42%;
    }
  }

@media screen and (max-width: 1024px){
    .filters{
      width: 62%;
    }
  }

</style>
