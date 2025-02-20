<template>
  <div class="cart">
    <Navbar :updateKeranjang="keranjangs" />
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
              <li class="breadcrumb-item active" aria-current="page">Cart</li>
            </ol>
          </nav>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col">
          <h2>
            <span class="font-weight-normal">My </span>
            <strong>Cart</strong>
          </h2>
          <div class="table-responsive mt-4">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">No</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Food Name</th>
                  <th scope="col">Information</th>
                  <th scope="col">Count</th>
                  <th scope="col">Price</th>
                  <th scope="col">Amount</th>
                  <th scope="col">Delete</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>
                    {{ index + 1 }}
                  </th>
                  <td>
                    <img
                      :src="'../assets/images/' + keranjang.products.gambar"
                      :alt="keranjang.gambar"
                      class="img-fluid"
                      width="250"
                    />
                  </td>
                  <td>
                    <strong>
                      {{ keranjang.products.nama }}
                    </strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>
                    {{ keranjang.jumlah_pemesanan }}
                  </td>
                  <td>
                    {{ formatCurrency(keranjang.products.harga) }}
                  </td>
                  <td>
                    <strong>
                      {{
                        formatCurrency(
                          keranjang.products.harga * keranjang.jumlah_pemesanan
                        )
                      }}
                    </strong>
                  </td>
                  <td>
                    <button
                      class="btn btn-danger"
                      @click="deleteKeranjang(keranjang.id)"
                    >
                      <b-icon-trash></b-icon-trash>
                    </button>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong> Total Harga : </strong>
                  </td>
                  <td colspan="1" align="right">
                    <strong>
                      {{ formatCurrency(totalHarga) }}
                    </strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
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
import Swal from "sweetalert2";
export default {
  name: "Cart",
  components: {
    Navbar,
    FooterVue,
  },
  data() {
    return {
      keranjangs: [],
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    formatCurrency(value) {
      if (!value) return "Rp. 0";
      return new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR",
        minimumFractionDigits: 0,
      }).format(value);
    },
    // deleteKeranjang(id) {
    //   console.log(id)
    //   axios
    //     .delete("http://kulineran-backend.wayanberdyanto.website/keranjangs.php/" + id)
    //     .then(() => {
    //       const Toast = Swal.mixin({
    //         toast: true,
    //         position: "top-end",
    //         showConfirmButton: false,
    //         timer: 3000,
    //         timerProgressBar: true,
    //         didOpen: (toast) => {
    //           toast.onmouseenter = Swal.stopTimer;
    //           toast.onmouseleave = Swal.resumeTimer;
    //         },
    //       });
    //       Toast.fire({
    //         icon: "success",
    //         title: "Delete Successfully",
    //       });
    //       axios
    //         .get("http://kulineran-backend.wayanberdyanto.website/keranjangs.php")
    //         .then((response) => {
    //           console.log("Berhasil", response);
    //           this.setKeranjangs(response.data);
    //         })
    //         .catch(function (error) {
    //           console.log("gagal", error);
    //         });
    //     })
    //     .catch(function (error) {
    //       // handle error
    //       console.log("gagal", error);
    //     });
    // },
    deleteKeranjang(id) {
      console.log("ID yang akan dihapus:", id);
      axios
        .delete(
          `http://kulineran-backend.wayanberdyanto.website/keranjangs.php/${id}`
        )
        .then((response) => {
          console.log("Response sukses:", response);
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
            title: "Berhasil Dihapus",
          });
          // Refresh data
          axios
            .get(
              "http://kulineran-backend.wayanberdyanto.website/keranjangs.php"
            )
            .then((response) => {
              this.setKeranjangs(response.data);
            })
            .catch((error) => {
              console.error("Gagal refresh:", error);
            });
        })
        .catch((error) => {
          console.error("Error detail:", error.response);
          Swal.fire({
            icon: "error",
            title: "Gagal Menghapus",
            text: error.response?.data?.error || "Terjadi kesalahan",
          });
        });
    },
  },
  mounted() {
    axios
      .get("http://kulineran-backend.wayanberdyanto.website/keranjangs.php")
      .then((response) => {
        // handle success
        console.log("Berhasil", response);
        this.setKeranjangs(response.data);
      })
      .catch(function (error) {
        // handle error
        console.log("gagal", error);
      });
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style></style>
