<template>
  <section class="container">
    <div>
      <Logo/>

      <button
        class="Button"
        v-on:click="sortedArray">
        Sort name by {{nameSort}}</button>

        <button
        class="Button"
        v-on:click="filteredSessionBeer" v-if="!filtered">
        Filter Session Beers</button>

        <button
        class="Button"
        v-on:click="resetFilters" v-if="filtered">
        Reset filters</button>

        <input v-model="searchValue" class="border-2 border-grey-100" />
        <button
        class="Button"
        v-on:click="searchBeers">
        Search beers</button>

        <button
        class="Button"
        v-on:click="resetFilters" v-if="searched">
        X</button>

     
      <div class="links grid gap-4 grid-cols-3">
        <nuxt-link
          v-for="beer in beers"
          :to="{name: 'beers-id', params: {id: beer.id}}"
          :key="beer.id"
          class="w-full"
        >
          {{beer.name}}
          <img :src='beer.image_url' :alt='beer.name' />
        </nuxt-link>
      </div>
    </div>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'

export default {
  components: {
    Logo
  },
  head () {
    return {
      title: 'Home Page 🍕',
      meta: [
        { name: 'twitter:title', content: 'Nuxt Async by Vue School'},
        { name: 'twitter:description', content: 'Nuxt + Vue School = 🍕'},
        { name: 'twitter:image', content: 'https://i.imgur.com/UYP2umJ.png'},
        { name: 'twitter:card', content: 'summary_large_image'}
      ]
    }
  },
  data() {
    return {
      nameSort: "Ascending",
      filtered: false,
      allBeers: [],
      searchValue: '',
      searched: false,
    }
  },
  async asyncData (context) {
    let response = await axios.get('https://api.punkapi.com/v2/beers')
    console.log(response.data, 'response');
    return {beers: response.data}
  },

  methods: {
    sortedArray: function() {
            let sortedData = this.beers;
            if(this.nameSort === "Ascending") {
                sortedData = sortedData.sort((a,b) => {
                    let fa = a.name.toLowerCase(), fb = b.name.toLowerCase();
                    if (fa < fb) {
                        return -1
                    }
                    if (fa > fb) {
                        return 1
                    }
                    
                    return 0
                })
                this.nameSort = "Descending"
            }
            else {
                sortedData.reverse();
                this.nameSort = "Ascending"
            }
            this.beers = sortedData;
        },
    filteredSessionBeer() {
      this.allBeers = this.beers;
      let sessionBeer = this.beers
      
      sessionBeer = sessionBeer.filter((item) => {
        return (item.abv <= 5);
      })
      this.filtered = true;
      this.beers = sessionBeer;
    },
    resetFilters() {
      this.beers = this.allBeers;
      this.filtered = false;
      this.searched = false;
      this.searchValue = '';
    },
    searchBeers() {
      this.allBeers = this.beers;
      let searchBeers = this.beers
      
      if (this.searchValue != '' && this.searchValue) {
            searchBeers = searchBeers.filter((item) => {
              return item.name
                .toUpperCase()
                .includes(this.searchValue.toUpperCase())
            })
          }
        this.beers = searchBeers;
        this.searched = true;
      }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
  'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  letter-spacing: 1px;
}
.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
}
.links {
  padding-top: 15px;
}
.links > .button--grey {
  margin: 5px;
}
</style>
