<template>
  <div>
    <Banner />
    <div class="main-container">
    <h1 class="custom-heading">Cadastrar Envio:</h1>
    <BurgerForm />
  </div>
  </div>
</template>

<script>
import Banner from '../components/Banner'
import BurgerForm from '../components/BurgerForm'

export default {
  name: 'Home',
  components: {
    Banner,
    BurgerForm
  }
}
</script>
<style scoped>
.main-container h1.custom-heading {
  color: #1b0d99;
}
</style>