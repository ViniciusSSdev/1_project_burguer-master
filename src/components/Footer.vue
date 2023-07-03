<template>
  <div id="app">
    <div id="content">
      <!-- Conteúdo principal da página aqui -->
    </div>
    <div id="footer">
      <p>Sãoroque &copy; 2023</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Footer",
};
</script>

<style scoped>
#app {
  display: flex;
  flex-direction: column;
  min-height: 450px /* Define a altura mínima da página para ocupar pelo menos 100% da altura da viewport */
}

#content {
  flex: 1; /* Ocupa todo o espaço disponível na área principal */
}

#footer {
  height: 80px;
  background-color: #3a2f99;
  border-top: 10px solid #8d1b1b;
  color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  border-top-left-radius: 30px; /* Arredondar a borda inferior esquerda */
  border-top-right-radius: 30px; /* Arredondar a borda inferior direita */
  font-weight:bold;
  
}
</style>
