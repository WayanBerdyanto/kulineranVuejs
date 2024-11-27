<template>
  <div class="card shadow card-product">
    <img
      :src="'assets/images/' + product.gambar"
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
      <a href="#" class="btn btn-success">
       Order
       <b-icon-cart></b-icon-cart>
      </a>
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