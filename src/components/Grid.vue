<template>
  <div class="grid">
    <div v-for="product in products" :key="product.photo">
      <div class="card">
        <img :src="require('@/assets/' + product.photo)"/>
        <p>{{ product.article }}</p>
        <p>{{ product.sexe }}</p>
        <p>{{ product.sport }}</p>
        <p>{{ product.couleur }}</p>
        <p>{{ product.prix }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import json from "../produits.json";

export default {
  name: 'Grid',
  props: {
    msg: String
  },
  data() {
    return {
      products: json,
    };
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  img{
    width: 100%;
  }
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 25px;
    position: -webkit-sticky;
    position: sticky;
  }
  .card{
    width: 100%;
  }
  @media screen and (max-width: 768px){
    .grid{
      grid-template-columns: 1fr 1fr;
    }
  }
</style>
