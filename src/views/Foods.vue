<template>
  <div class="foods">
    <Navbar/>
    <div class="container">
      <h1>Teest
        
      </h1>
    </div>
    <FooterVue />
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import FooterVue from "@/components/Footer.vue"
export default {
    name: "FoodViews",
    components: {
      Navbar,
      FooterVue
    }
}
</script>

<style>

</style>