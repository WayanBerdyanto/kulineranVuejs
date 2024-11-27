<template>
  <div class="home">
    <NavbarVue />
    <div class="container">
      <Hero />

      <div class="row mt-4">
        <div class="col">
          <h2>
            <span class="font-weight-normal">Best</span>
            <strong>Foods</strong>
          </h2>
        </div>
        <div class="col">
          <router-link to="/foods" class="btn btn-success float-right">
            See All
            <b-icon-eye></b-icon-eye>
          </router-link>
        </div>
      </div>

      <div class="row mb-4">
        <div
          class="col-md-4 mt-4"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
    <FooterVue />
  </div>
</template>

<script>
import NavbarVue from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
import CardProduct from "@/components/CardProduct.vue";
import FooterVue from "@/components/Footer.vue"
import axios from "axios";

// @ is an alias to /src
export default {
  name: "HomeView",
  components: {
    NavbarVue,
    Hero,
    CardProduct,
    FooterVue
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/best-products")
      .then((response) => {
        // handle success
        console.log("Berhasil", response);
        this.setProduct(response.data);
      })
      .catch(function (error) {
        // handle error
        console.log("gagal", error);
      });
  },
};
</script>
