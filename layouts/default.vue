<template>
    <div class="container mx-auto px-5">
        <AppHeader />
        <Nuxt />
        <AppFooter />
    </div>
  </template>

<script>
import AppHeader from '~/components/AppHeader.vue';
import AppFooter from '~/components/AppFooter.vue';

export default {
    components: { AppHeader, AppFooter }
}
</script>

<style>
    body { background: lightgray; }
</style>