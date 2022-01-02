<template>
  <section class="container">
    <div>
      <Logo/>
      
      <div class="flex flex-row w-full justify-center">
        <button
        class="Button font-bold u-text-lg uppercase mr-4"
        v-on:click="sortedArray">
        Sort name by {{nameSort}}</button>

        <input v-model="searchValue" class="border-2 border-grey-100" />
        <button
        class="Button border-2 font-bold p-2 text-sm uppercase"
        v-on:click="searchBeers">
        Search beers</button>

        <button
        class="Button border-2"
        v-on:click="resetFilters" v-if="searched">
        X</button>
      </div>
      

        

     

        <div class="flex flex-col lg:flex-row">
          <div class="w-4/12">
            <h3>Alchol Percentage</h3>
            <input type="radio" id="alcholpercent1" name="alcholpercent" value="3.5" @change="alcholPercentFilter($event)">
            <label for="alcholpercent">3.5%</label><br>
            <input type="radio" id="alcholpercent2" name="alcholpercent" value="4.5" @change="alcholPercentFilter($event)">
            <label for="alcholpercent">4.5%</label><br>
            <input type="radio" id="alcholpercent3" name="alcholpercent" value="5.5" @change="alcholPercentFilter($event)">
            <label for="alcholpercent">5.5%</label><br>

            <button
            class="Button border-2"
            v-on:click="filteredSessionBeer" v-if="!filtered">
            Filter Session Beers</button>

            <button
            class="Button border-2"
            v-on:click="resetFilters" v-if="filtered">
            Reset filters</button>
          </div>

          <div class="links grid gap-14 grid-cols-3">
            <nuxt-link
              v-for="beer in beers"
              :to="{name: 'beers-id', params: {id: beer.id}}"
              :key="beer.id"
              class="w-full flex flex-col justify-end bg-green-100 p-6 mb-6 h-full hover:opacity-60 group"
            >
              <div>
                <img :src='beer.image_url' :alt='beer.name' class="beer-img mx-auto mb-4" />
                <h3 class="font-bold text-4xl uppercase mt-0" v-if="beer.name" v-html="beer.name" />
                <div class="uppercase border-4 bg-transparent text-black border-purple-900 group-hover:bg-purple-900 group-hover:text-white font-bold text-4xl my-6 p-4">Drink me</div>
              </div>
            </nuxt-link>
          </div>
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
      },
      alcholPercentFilter(event) {
        this.allBeers = this.beers;
        let filteredBeers = this.allBeers;

        var radioChoice = event.target.value;

         filteredBeers = filteredBeers.filter((item) => {
            return (item.abv <= radioChoice);
          })
          this.filtered = true;
          this.beers = filteredBeers;
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
  margin-left: auto;
  margin-right: auto;
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
.beer-img {
  max-width: 200px;
}
</style>
