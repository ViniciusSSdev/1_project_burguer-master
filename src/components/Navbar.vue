<template>
  <router-link id="logo-url" to="/"></router-link>
  
  <div>
    <div id="nav">
      <router-link to="/">
        <img id="logo" :src="logo" :alt="alt">
      </router-link>
      <router-link to="/">Cadastrar Envios</router-link>
      <router-link to="/pedidos">Ver Envios</router-link>
      <router-link to="/finalizados">Finalizados</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  props: ["logo", "alt"]
}
</script>

<style scoped>
#nav {
  background-color: #3a2f99;
  border-bottom: 20px solid #8d1b1b;
  padding: 15px 50px;
  display: flex;
  height: 110px;
  justify-content: flex-end;
  align-items: center;
  font-weight: bold;
  border-bottom-left-radius: 30px;
  border-bottom-right-radius: 30px;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
}

#nav #logo-url {
  margin: 0;
  margin-left: 0;
}

#nav #logo {
  max-width: 100%;
  height: 260px;
  width: 340px;
  margin-right: 450px;
  margin-top: 150px;
}

#nav a {
  color: #f5f5f5;
  text-decoration: none;
  margin: 12px;
  transition: .5s;
}

#nav a:hover {
  color: #b90303;
}
</style>
