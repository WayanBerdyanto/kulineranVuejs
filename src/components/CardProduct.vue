<template>
  <div class="card shadow card-product">
    <img
      :src="'http://kulineran-backend.wayanberdyanto.website/assets/' + product.gambar"
      class="card-img-top"
      :alt="product.gambar"
      width="100%"
      height="250"
    />
    <div class="card-body">
      <h5 class="card-title">
        {{ product.nama }}
      </h5>
      <p class="card-text">
        Price : Rp {{ product.harga }}
      </p>
      <router-link :to="'/foods/' + product.id" class="btn btn-success">
       Order
       <b-icon-cart></b-icon-cart>
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["product"],
};
</script>

<style>
</style>