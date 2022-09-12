<template>
<nuxt-link :to="'/'+ id">
  <div class="px-5 py-5 mb-5 border bg-white">
    <p class="text-sm font-bold uppercase mb-3">{{ listingTypes[0].title }}</p>
    <img class="mb-5" :src="image_url" alt="">
    <h1 class="text-xl font-bold">{{ title | uppercase }}</h1>
    <p class="mb-5">{{ address }}</p>
    <div class="flex justify-between">
        <div>Bedrooms: {{ bedrooms }}</div>
        <div>Bathrooms: {{ bathrooms }}</div>
    </div>
  </div>
  </nuxt-link>
</template>

<script>
export default {
    name:'home',
    props:["id", 'title', 'address', 'listingTypes', 'coverId', 'bedrooms', 'bathrooms'],
    data() {
        return {
            image_url:"https://placehold.jp/250x250.png"
        }
    },
    filters: {
        uppercase(value) {
            return value.toUpperCase()
        }
    },
    async created(){
        try {
            const api_media_url = "https://homehapp-api.jsteam.gaussx.com/api/media/" + this.$props.coverId
            const res_img = await fetch(api_media_url)
            this.image_url = await res_img.url
        } catch (err) {
            console.log(err)
        }

    }

}
</script>

<style>

</style>
