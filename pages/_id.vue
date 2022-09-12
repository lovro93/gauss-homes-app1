<template>
    <div class="px-5 py-5 mb-5 border bg-white">
      <p class="text-sm font-bold uppercase mb-3">{{ listingTypes }}</p>
      <img class="w-1/2 mb-5" :src="image_url" alt="">
      <h1 class="text-xl font-bold">{{ title }}</h1>
      <p>{{ address }}</p>
    </div>
</template>
  
 <script>
  export default {
    name:'home',
    data(){
        return{
            title:"",
            address:"",
            listingTypes:"",
            image_url:""
        }
    },
    async created(){
        console.log(this.$route.params.id)
        const api_url = "https://homehapp-api.jsteam.gaussx.com/api/home/" + this.$route.params.id
        const res = await fetch(api_url)
        const result = await res.json()
        //console.log(result.data);

        this.title = result.data.title
        this.address = result.data.address
        this.listingTypes = result.data.listingTypes[0].title

        const api_media_url = "https://homehapp-api.jsteam.gaussx.com/api/media/" + result.data.cover.id
        const res_img = await fetch(api_media_url)
        this.image_url = await res_img.url
    }
  }
  
</script>

<style>

</style>