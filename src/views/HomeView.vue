<template>
  <div class="home">
    <NavbarVue />
    <div class="container">
      <Hero />
    </div>
  </div>
</template>

<script>
import NavbarVue from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
// @ is an alias to /src
export default {
  name: "HomeView",
  components: {
    NavbarVue,
    Hero,
  },
};
</script>
