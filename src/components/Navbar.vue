<template>
  <nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <router-link class="navbar-item" to="/">
        <img src="../assets/images/carrito.png" width="45">
      </router-link>

    </div>

    <div class="navbar-menu">
      <div class="navbar-start">
        <router-link class="navbar-item" to="/">
          <a class="navbar-item" :class="{'navbar-active': currentRouteName === 'home'}">
            <strong>Inicio</strong>
          </a>
        </router-link>

        <router-link class="navbar-item" to="/catalogo">
          <a class="navbar-item" :class="{'navbar-active': currentRouteName === 'catalogo'}">
            <strong>Catálogo</strong>
          </a>
        </router-link>

        <router-link class="navbar-item" to="/producto">
          <a class="navbar-item" :class="{'navbar-active': currentRouteName === 'producto'}">
            <strong>Producto</strong>
          </a>
        </router-link>

        <router-link class="navbar-item"  to="/about">
          <a class="navbar-item" :class="{'navbar-active': currentRouteName === 'about'}">
            <strong>Acerca de</strong>
          </a>
        </router-link>

      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  computed: {
    currentRouteName() {
      console.log(this.$route.name);
      return this.$route.name;
    }
  }
}
</script>

<style scoped>
.navbar-active {
  color: #485fc7;
}
</style>
