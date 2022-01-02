<template>
  <div class="container">
    <Logo />
    <article>
      <Beer :title="beer.name" :imageUrl="beer.image_url" :description="beer.description" :tagline="beer.tagline" />
      <div class="p-8 mx-14 border-2 inline-block">
        <h4 class="font-bold text-3xl mt-0 text-gray-400 uppercase">Food pairing</h4>
        <p v-for="pairing in beer.food_pairing"
              v-html="pairing"
              :key="pairing" />
      </div>
    </article>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    head () {
      return {
        title: this.beer.name,
        meta: [
          { name: 'twitter:title', content: this.beer.title},
          { name: 'twitter:description', content: this.beer.body},
          { name: 'twitter:image', content: 'https://i.imgur.com/UYP2umJ.png'},
          { name: 'twitter:card', content: 'summary_large_image'}
        ]
      }
    },
    data () {
      return {
        id: this.$route.params.id
      }
    },
    async asyncData ({params}) {
      let response = await axios.get(`https://api.punkapi.com/v2/beers/${params.id}`)
      console.log(response.data[0], 'data response');
      return {beer: response.data[0]}
    }
  }
</script>

<style scoped>
  .container {
    display: flex;
    justify-content: space-between;
    line-height: 1.5;
  }
  article * {
    margin-bottom: 1rem;
  }
  aside {
    min-width: 280px;
    max-width: 280px;
    padding-left: 2rem;
  }
  .title {
    font-size: 2rem;
  }
</style>
