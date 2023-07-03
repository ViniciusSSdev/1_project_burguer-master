<template>
  <div id="burger-table" v-if="finalizados">
    
    <div class="main-container-header">
      <h1 class="custom-heading">Envios finalizados</h1>
      <div id="filter-section">
        <label for="date-filter">Filtrar por Data:</label>
        <input type="text" id="date-filter" v-model="dateFilter" @input="filterByDate" placeholder="ANO-MES-DIA" />
        <label id="search" for="search">Pesquisar Envio:</label>
        <input type="text" v-model="searchTerm" @input="filterBySearch" placeholder="Pesquisar...">
      </div>
      <BurgerForm @burgerCreated="getPedidos" />
    </div>
    <div id="burger-table-heading">
      <div class="order-id">N°</div>
      <div>Solicitante:</div>
      <div>Motorista:</div>
      <div>Filial:</div>
      <div>Data do Envio:</div>
      <div>Detalhes do Envio:</div>
      <div>N° do Chamado:</div>
      <div>N° do Patrimônio:</div>
      <div>Ações:</div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="finalizado in filteredFinalizados" :key="finalizado.id">
        <div class="order-number">{{ finalizado.id }}</div>
        <div>{{ finalizado.nome }}</div>
        <div>{{ finalizado.motorista }}</div>
        <div>{{ finalizado.filial }}</div>
        <div>{{ finalizado.data }}</div>
        <div>
          <ul v-for="(opcional, index) in finalizado.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>{{ finalizado.num }}</div>
        <div>{{ finalizado.patrimonio }}</div>
        <button @click="deleteBurger(finalizado.id)">Excluir</button>
      </div>
    </div>
    
  </div>
  <div v-else>
    <h2>Não há envios no momento!</h2>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  data() {
    return {
      finalizados: null,
      finalizado_id: null,
      selectedStatus: "",
      dateFilter: "",
      searchTerm: ""
    };
  },
  computed: {
    filteredFinalizados() {
      let filteredData = this.finalizados;

      if (this.selectedStatus !== "") {
        filteredData = filteredData.filter(
          finalizado => finalizado.status === this.selectedStatus
        );
      }

      if (this.dateFilter !== "") {
        filteredData = filteredData.filter(finalizado =>
          finalizado.data.includes(this.dateFilter)
        );
      }

      const searchTerm = this.searchTerm.toLowerCase().trim();
      if (searchTerm) {
        filteredData = filteredData.filter(finalizado => {
          return (
            finalizado.nome.toLowerCase().includes(searchTerm) ||
            finalizado.motorista.toLowerCase().includes(searchTerm) ||
            finalizado.filial.toLowerCase().includes(searchTerm) ||
            finalizado.num.includes(searchTerm) ||
            finalizado.patrimonio.includes(searchTerm)
          );
        });
      }

      return filteredData;
    }
  },
  methods: {
    async getPedidos() {
      const req = await fetch('http://localhost:3000/finalizados');
      const data = await req.json();
      this.finalizados = data;
    },
    async deleteBurger(id) {
      const req = await fetch(`http://localhost:3000/finalizados/${id}`, {
        method: "DELETE"
      });
      if (req.ok) {
        this.getPedidos();
      } else {
        console.error("Erro ao excluir o envio.");
      }
    },
    filterByDate() {
      this.selectedStatus = "";
    },
    filterBySearch() {
      this.selectedStatus = "";
    }
  },
  mounted() {
    this.getPedidos();
  }
};
</script>

<style scoped>
#burger-table {
  max-width: 100%;
  margin: 0 auto;
  margin-top: 170px;
  margin-left: 30px;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

.burger-table-row {
  width: 100%;
  padding: 15px;
  border-bottom: 1px solid #ccc;
}

#burger-table-heading div,
.burger-table-row div {
  width: 12%;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

#filter-section {
  margin-top: 20px;
}

#filter-section label {
  margin-right: 10px;
}

#filter-section input {
  padding: 6px;
}

.main-container-header h1.custom-heading {
  color: #1b0d99;
  margin-right: 200px;
  margin-top: 180px;
}
#search{
margin-left: 20px;
}
</style>
