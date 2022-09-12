<template>
<div>
  <div class="flex justify-start space-x-2 mb-5">

        <button @click="filter(2)" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            For Rent
        </button>
        <button @click="filter(1)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded">
            For Sale
        </button>   

        <input @keydown.enter="search(term)" v-model="term" type="text">
  

        <button class="bg-white font-bold py-2 px-4 rounded" @click="prevpage()" v-if="filters.page > 1">&lt; Prev</button>
        <button class="bg-white font-bold py-2 px-4 rounded" @click="nextpage()" v-if="filters.page < lastPage">Next ></button>
  
  </div>
  <div class="grid xl:grid-cols-4 lg:grid-cols-3 md:grid-cols-2 sm:grid-cols-1 gap-4">
    <Home 
      v-for="home in homes" 
      :key="home.id" 
      :coverId="home.cover.id" 
      :id="home.id" 
      :title="home.title" 
      :address="home.displayAddress" 
      :listingTypes="home.listingTypes"
      :bedrooms="home.bedrooms"
      :bathrooms="home.bathrooms" 
    />
  </div>

</div>
</template>

<script>
import Home from '~/components/Home.vue';
import debounce from 'loadsh'
export default {
    name: "IndexPage",
    data(){
      return{
        homes: [],
        pagination: null,
        term:'',
        filters: {
          page: null,
          searchText: null,
          listingTypes: null,
          bathrooms: null,
          bedrooms: null,
        },
      }
    },
    watch: {
      '$route.query': {
        handler: async function() {
           this.readQueryParams()
           await this.fetchHomes()
        },
        deep: true
      },
      'filters': {
        handler: async function () { 
           this.readQueryParams()
           await this.fetchHomes()
          },
        deep: true,
        immediate: true   
      }
    },
    async created(){
      this.readQueryParams()
      await this.fetchHomes()
    },

    computed: {
      // a computed getter
      lastPage() {
        // `this` points to the component instance

        return (this.pagination) ? this.pagination.lastPage : 0
      }
    },
    methods:{
      nextpage() {
        // this.$router.replace( {query: {page: this.filters.page + 1}})
        this.$router.push({ query: Object.assign({}, this.$route.query, { page: this.filters.page + 1 }) });
      },
      prevpage() {
        this.$router.push({ query: Object.assign({}, this.$route.query, { page: this.filters.page - 1 }) });
      },
      readQueryParams() {
        const { page, searchText, listingTypes, bathrooms, bedrooms } =
          this.$route.query;

        const { filters } = this;
        filters.page = +page || 1;
        filters.searchText = searchText || null;
        filters.listingTypes = +listingTypes || 1;
        filters.bathrooms = +bathrooms || null;
        filters.bedrooms = +bedrooms || null;
      },
      async fetchHomes() {
        try {
          const res = await this.$axios.$get("https://homehapp-api.jsteam.gaussx.com/api/home", {
            params: {
              ...this.filters,
            },
          });

          this.homes = res.data.data;
          this.pagination = res.data.pagination;
        } catch (e) {
          const message =
            e?.response?.data?.message || "Error while fetching properties";
          console.error(message);
        }
      },
      filter(type) {
          this.filters.listingTypes = type
          this.$router.push({ query: Object.assign({}, this.$route.query, { listingTypes: type,page: 1 }) });

      },
      search(text) {
        this.filters.searchText = text
        this.term = ''
        this.$router.push({ query: this.filters })      
      }
    }  
  }
      
     
</script>

<style>

</style>
