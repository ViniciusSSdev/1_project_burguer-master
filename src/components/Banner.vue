<template>
  <div id="main-banner">
    <h1>Controle de envio T.I</h1>
  </div>
</template>

<script>
  export default {
    name: "Banner"
  }
</script>

<style scoped>

  #main-banner {
  background-image: url('../components/burger.jpg');
  background-position: 0 -500px;
  height: 350px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  z-index: 0; /* Define a ordem de empilhamento para ficar abaixo dos demais elementos */
  width: 100%;
  margin-top: 80px;
}

  #main-banner h1 {
    color: #FFF;
    text-align: center;
    font-size: 60px;
    background-color: #33439e38;
    padding: 20px 40px;
  }

</style>