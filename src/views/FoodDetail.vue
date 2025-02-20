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
            :src="
              'http://kulineran-backend.wayanberdyanto.website/assets/' +
              product.gambar
            "
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

          <form class="mt-4" v-on:submit.prevent>
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
                  readonly
                  v-model="pesan.jumlah_pemesanan"
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
                v-model="pesan.keterangan"
                id="information"
                class="form-control"
                placeholder="example: Spicy"
              ></textarea>
            </div>

            <button type="submit" class="btn btn-success" @click="pemesanan">
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
import { ref } from "vue";
import Swal from "sweetalert2";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
    FooterVue,
  },
  setup() {
    const pesan = ref({
      jumlah_pemesanan: 0,
      keterangan: "",
    });

    const increment = () => {
      pesan.value.jumlah_pemesanan++;
    };

    const decrement = () => {
      if (pesan.value.jumlah_pemesanan > 1) {
        pesan.value.jumlah_pemesanan--;
      }
    };

    return {
      pesan,
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
    pemesanan() {
      this.pesan.products = this.product;
      if (this.pesan.jumlah_pemesanan) {
        const payload = {
          product_id: this.product.id, // Pastikan mengirim product_id
          jumlah_pemesanan: this.pesan.jumlah_pemesanan,
          keterangan: this.pesan.keterangan || "",
        };
        axios
          .post(
            "http://kulineran-backend.wayanberdyanto.website/keranjangs.php",
            payload,
            {
              headers: {
                "Content-Type": "application/json",
              },
            }
          )
          .then(() => {
            this.$router.push({ path: "/cart" });
            const Toast = Swal.mixin({
              toast: true,
              position: "top-end",
              showConfirmButton: false,
              timer: 3000,
              timerProgressBar: true,
              didOpen: (toast) => {
                toast.onmouseenter = Swal.stopTimer;
                toast.onmouseleave = Swal.resumeTimer;
              },
            });
            Toast.fire({
              icon: "success",
              title: "Order Successfully",
            });
          })
          .catch((err) => console.log(err));
      } else {
        const Toast = Swal.mixin({
          toast: true,
          position: "top-end",
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          didOpen: (toast) => {
            toast.onmouseenter = Swal.stopTimer;
            toast.onmouseleave = Swal.resumeTimer;
          },
        });
        Toast.fire({
          icon: "error",
          title: "Please Input Data",
        });
      }
    },
  },
  mounted() {
    const productId = this.$route.params.id;
    axios
      .get(
        `http://kulineran-backend.wayanberdyanto.website/products.php?id=${productId}`
      )
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

<style></style>
