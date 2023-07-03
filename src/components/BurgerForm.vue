<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <form id="burger-form" method="POST" @submit="createEnvio">
      <div class="flex-container">
    <div class="input-container">
      <label for="nome">Nome do solicitante:</label>
      <input type="text" id="nome" name="nome" v-model="nome" @input="validateInput('nome')" placeholder="Digite o solicitante" required :class="{ error: nomeError }">
      <div v-if="nomeError" class="error-message">Por favor, digite apenas letras.</div>
    </div>

    <div class="input-container">
      <label for="motorista">Nome do motorista:</label>
      <input type="text" id="motorista" name="motorista" v-model="motorista" @input="validateInput('motorista')" placeholder="Digite o nome do motorista" required :class="{ error: motoristaError }">
      <div v-if="motoristaError" class="error-message">Por favor, digite apenas letras.</div>
    </div>
  </div>
  <div class="flex-container">
    <div class="input-container">
      <div class="input-container-high">
        <label for="num">Número do Chamado:</label>
        <input type="text" id="num" name="num" v-model="num" @input="validateInput('num')" placeholder="Digite o Número do Chamado" style="margin-top: 20px;" required :class="{ error: numError }">
        <div v-if="numError" class="error-message">Por favor, digite apenas números.</div>
      </div>
    </div>
    <div class="input-container">
      <div class="input-container-high">
        <label for="patrimonio">Número do Patrimônio:</label>
        <input type="text" id="patrimonio" name="patrimonio" v-model="patrimonio" @input="validateInput('patrimonio')" placeholder="Digite o Número do patrimônio" style="margin-top: 20px;" required :class="{ error: patrimonioError }">
        <div v-if="patrimonioError" class="error-message">Por favor, digite apenas números.</div>
      </div>
    </div>
  </div>
  <div class="flex-container">
    <div class="input-container">
      <label for="filial">Filial de Destino:</label>
      <select name="filial" id="filial" v-model="filial" required>
        <option disabled selected value="">Escolha uma filial</option>
        <option v-for="filial in filiais" :key="filial.id" :value="filial.tipo">{{ filial.tipo }}</option>
      </select>
    </div>
    <div class="input-container">
      <div class="input-container-high">
        <label for="desc">Descrição do envio:</label>
        <input type="text" id="desc" name="desc" v-model="desc" placeholder="Detalhes do produto enviado" style="margin-top: 20px;">
      </div>
    </div>
  </div>

  <div class="flex-container">
    <div class="input-container">
      <label for="data">Data de chegada ao transporte:</label>
      <input type="date" id="data" name="data" v-model="data" required>
    </div>

    <div id="opcionais-container" class="input-container">
      <label id="opcionais-title" for="opcionais">Equipamentos enviados:</label>
      <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
        <span>{{ opcional.tipo }}</span>
      </div>
    </div>
  </div>
  <div class="input-container">
    <input class="submit-btn" type="submit" value="Cadastrar Envio">
  </div>
</form>

  </div>
</template>



<script>
import Message from './Message'

export default {
  name: "BurgerForm",
  components: {
    Message
  },
  data() {
    return {
      filiais: null,
      motoristas: null,
      opcionais: [],
      opcionaisdata: null,
      nome: null,
      nomeError: false,
      filial: null,
      patrimonio: null,
      patrimonioError: false,
      motorista: null,
      motoristaError: false,
      num: null,
      numError: false,
      data: null, // Campo de data
      status: "Solicitado",
      msg: null
    }
  },
  methods: {
    validateInput(field) {
      const regex = /^[a-zA-Z\s]*$/; // Apenas letras e espaços são permitidos
      if (field === 'nome') {
        this.nomeError = !regex.test(this.nome);
      } else if (field === 'motorista') {
        this.motoristaError = !regex.test(this.motorista);
      }
    },validateInput(field) {
      const regex = /^[0-9]*$/; // Apenas números são permitidos
      if (field === 'num') {
        this.numError = !regex.test(this.num);
      } else if (field === 'patrimonio') {
        this.patrimonioError = !regex.test(this.patrimonio);
      }
    },
    async getIngredientes() {
      const req = await fetch('http://localhost:3000/ingredientes')
      const data = await req.json()

      this.filiais = data.filiais
      this.motoristas = data.motoristas
      this.opcionaisdata = data.opcionais
      this.selectedDate = data.selectedDate
    },
    async createEnvio(e) {

      e.preventDefault()

      const data = {
        nome: this.nome,
        motorista: this.motorista,
        filial: this.filial,
        opcionais: Array.from(this.opcionais),
        num: this.num,
        data: this.data,
        patrimonio: this.patrimonio,
        status: "Solicitado",
        data: this.data // Adicione o valor do campo de data ao objeto de dados
      }

      const dataJson = JSON.stringify(data)

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson
      });

      const res = await req.json()

      console.log(res)

      this.msg = "Envio cadastrado com sucesso!"

      // clear message
      setTimeout(() => this.msg = "", 3000)

      // limpar campos
      this.nome = ""
      this.motorista = ""
      this.filial = ""
      this.numm = ""
      this.opcionais = [],
      this.data = "";
      this.patrimonio = "" // Limpe o campo de data

    }
  },
  mounted() {
    this.getIngredientes()
  },
  components: {
    Message
  }
}
</script>


<style scoped>
.flex-container {
  display: flex;
  margin-bottom: 20px;
  /* Espaçamento entre as linhas */

  

}

.flex-container>.input-container {
  flex: 1;
  margin-right: 50px;


}

#burger-form {
  max-width: 400px;
  margin: auto 600px;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px
}

.input-container input::placeholder {
  font-weight: bold;

}

label {
  margin-bottom: 10px;
  color: #222;
  ;
  padding: 5px 10px;
  border-left: 4px solid #1b0d99;
  font-size: 23px;

}

input,
select {
  padding: 5px 2px;
  width: 300px;
  color: #161515;
  font-size: 15px;
}

#opcionais-container {
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  margin-bottom: 10px;


}

.checkbox-container span,
.checkbox-container input {
  width: 25px;
}

.checkbox-container span {
  
  margin-right: 10px;
  font-weight: bold;
}

.submit-btn {
  background-color: #00830b;
  color: #fdfdfd;
  border: 2px solid #222;
  padding: 10px;
  font-size: 20px;
  cursor: pointer;
  transition: .7s;
  border-radius: 20px;
  margin-left: 125px;
}

.submit-btn:hover {
  background-color: transparent;
  color: #3a2f99;
}
.error {
  border: 1px solid red;
}

.error-message {
  color: red;
  font-size: 12px;
  margin-top: 4px;
}</style>