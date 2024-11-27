<template>
  <div class="detail-food">
    <Navbar />
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-4">
        <div class="col-md-6">
          <img
            :src="'../assets/images/' + product.gambar"
            :alt="product.gambar"
            class="img-fluid"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>
              {{ product.nama }}
            </strong>
          </h2>
          <hr />
          <h4>
            Price : Rp <strong> {{ product.harga }}</strong>
          </h4>

          <form action="">
            <div class="form-group mb-3">
              <label for="count_order">Count Order</label>
              <div class="input-group">
                <div class="input-group-prepend">
                  <span
                    class="input-group-text btn btn-danger"
                    @click="decrement"
                  >
                    <b-icon-dash></b-icon-dash>
                  </span>
                </div>
                <input
                id="count_order"
                  type="text"
                  class="form-control form-control-order"
                  aria-label="Amount (to the nearest dollar)"
                  :value="count"
                  readonly
                />
                <div class="input-group-append">
                  <span
                    class="input-group-text btn btn-primary"
                    @click="increment"
                  >
                    <b-icon-plus></b-icon-plus>
                  </span>
                </div>
              </div>
            </div>

            <div class="form-group mb-3">
              <label for="information">Information</label>
              <textarea
                id="information"
                class="form-control"
                placeholder="example: Spicy  "
              ></textarea>
            </div>

            <button type="submit" class="btn btn-success">
              Order
              <b-icon-cart></b-icon-cart>
            </button>
          </form>
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
import { ref, watch } from "vue";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
    FooterVue,
  },
  setup() {
    const count = ref(0);

    const increment = () => {
      count.value++;
    };

    const decrement = () => {
      count.value--;
    };

    watch(count, (newValue) => {
      if (newValue < 0) {
        count.value = 0;
      }
    });

    return {
      count,
      increment,
      decrement,
    };
  },

  data() {
    return {
      product: [],
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
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