<template>
  <div class="foods">
    <Navbar />
    <div class="container">
      <div class="row">
        <div class="col">
          <h2>
            <span class="font-weight-normal">List</span>
            <strong> Foods</strong>
          </h2>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input v-model="search"
              type="text"
              class="form-control"
              placeholder="Search Name"
              aria-label="Search Name"
              aria-describedby="basic-addon2"
              @keyup="searchFood"
            />
            <div class="input-group-append">
              <button class="btn btn-success" id="basic-addon2">
                <b-icon-search></b-icon-search>
              </button>
            </div>
          </div>
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
import Navbar from "@/components/Navbar.vue";
import FooterVue from "@/components/Footer.vue";
import axios from "axios";
import CardProduct from "@/components/CardProduct.vue";

export default {
  name: "FoodViews",
  components: {
    Navbar,
    FooterVue,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: '',
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    searchFood() {
      axios
        .get("http://localhost:3000/products?q="+this.search)
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
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
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

<style>
</style>