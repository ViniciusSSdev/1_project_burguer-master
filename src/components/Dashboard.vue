<template>
  <div id="burger-table" v-if="burgers">
    <div id="burger-filter">
      <label for="status-filter">Filtrar por Status:</label>
      <select id="status-filter" class="filter-select" v-model="selectedStatus" @change="filterByStatus">
        <option value="">Todos</option>
        <option v-for="statusOption in statusOptions" :value="statusOption">{{ statusOption }}</option>
      </select>
    </div>
    <div>
      <div id="burger-table-heading">
        <div class="order-id">N°</div>
        <div>Solicitante:</div>
        <div>Motorista:</div>
        <div>Filial:</div>
        <div>Data do Envio:</div>
        <div>Detalhes do Envio:</div>
        <div>N° do Chamado:</div>
        <div>N° do Patrimônio:</div>
        <div>Status:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in filteredBurgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.motorista }}</div>
        <div>{{ burger.filial }}</div>
        <div>{{ burger.data }}</div>
        <div>
          <ul v-for="(opcional, index) in burger.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>{{ burger.num }}</div>
        <div>{{ burger.patrimonio }}</div>
        <div>
          <select name="status" class="status" @change="updateBurger($event, burger.id)">
            <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="burger.status == s.tipo">
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="confirmDelete(burger.id)">Finalizado</button>
        </div>
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
      burgers: null,
      status: [],
      selectedStatus: "",
      finalizados: []
    };
  },
  computed: {
    filteredBurgers() {
      if (this.selectedStatus === "") {
        return this.burgers;
      } else {
        return this.burgers.filter(burger => {
          return burger.status === this.selectedStatus;
        });
      }
    },
    statusOptions() {
      return [...new Set(this.burgers.map(burger => burger.status))];
    }
  },
  methods: {
    async getPedidos() {
      const req = await fetch('http://localhost:3000/burgers');
      const data = await req.json();
      this.burgers = data;
      this.getStatus();
    },
    async getStatus() {
      const req = await fetch('http://localhost:3000/status');
      const data = await req.json();
      this.status = data;
    },
    confirmDelete(id) {
      if (window.confirm("Tem certeza de que deseja finalizar este Envio?")) {
        this.deleteBurger(id);
      }
    },
    async deleteBurger(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE"
      });

      if (req.ok) {
        const deletedBurger = this.burgers.find(burger => burger.id === id);
        this.finalizados.push(deletedBurger);

        this.burgers = this.burgers.filter(burger => burger.id !== id);

        const data = {
          nome: deletedBurger.nome,
          motorista: deletedBurger.motorista,
          filial: deletedBurger.filial,
          opcionais: deletedBurger.opcionais,
          num: deletedBurger.num,
          data: deletedBurger.data,
          patrimonio: deletedBurger.patrimonio,
          status: "Solicitado"
        };

        const dataJson = JSON.stringify(data);

        const postReq = await fetch("http://localhost:3000/finalizados", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: dataJson
        });

        if (postReq.ok) {
          console.log("Envio adicionado aos finalizados com sucesso!");
        } else {
          console.error("Erro ao adicionar o envio aos finalizados.");
        }
      } else {
        console.error("Erro ao excluir o envio.");
      }
    },
    async updateBurger(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({ status: option });
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson
      });
      const res = await req.json();
      console.log(res);
    },
    filterByStatus() {
      if (this.selectedStatus === "") {
        this.filteredBurgers = this.burgers;
      } else {
        this.filteredBurgers = this.burgers.filter(burger => {
          return burger.status === this.selectedStatus;
        });
      }
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
  margin-top: 60px;
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
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

#burger-table-heading div,
.burger-table-row div {
  width: 11%;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

#burger-filter {
  margin-bottom: 10px;
}

.filter-select {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #fff;
  color: #333;
  font-size: 14px;
  margin-left: 10px;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  flex-direction: row;
  background-color: #3a2f99;
  color: #f0f0f0;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
