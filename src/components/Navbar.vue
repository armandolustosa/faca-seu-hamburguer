<template>
  <nav id="nav">
    <router-link to="/" id="logo-url">
      <img :src="logo" :alt="alt" id="logo" />
    </router-link>
    <router-link to="/">Home</router-link> <span id="divisor">|</span>
    <router-link to="/pedidos">Pedidos</router-link>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  props: ["logo", "alt"],
};
</script>

<style scoped>
#nav {
  background-color: #F4A460;
  border-bottom: 4px solid #D2691E;
  padding: 15px 50px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

#nav > #logo-url {
  margin: auto;
  margin-left: 0;
}

#logo {
  width: 40px;
  height: 40px;
}

#nav > a {
  color: #F5DEB3;
  text-decoration: none;
  margin: 12px;
  font-weight: bold;
  transition: 0.5s;
}

#nav > a:hover {
  color: #D2691E;
  
}

#divisor {
  color: #D2691E;
}
</style>
