<template>
  <b-navbar toggleable="lg" type="light" variant="light" class="mb-4">
    <b-container>
      <b-navbar-brand href="#">Kulineran</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item to="/" tag="router-link">Home</b-nav-item>
          <b-nav-item to="/foods" tag="router-link">Foods</b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto">
          <b-nav-item to="/keranjang" tag="router-link" active-class="active">
            Cart
            <b-icon icon="cart"></b-icon>
            <b-badge variant="success" class="ml-2">0</b-badge>
          </b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-container>
  </b-navbar>
</template>

<script>
export default {
  name: "NavbarVue",
};
</script>
