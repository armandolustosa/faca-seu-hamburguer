<template>
  <div>
    <!-- Componente de mensagem -->
    <Message :msg="msg" v-show="msg" />
    <div>
      <form id="burger-form" @submit="createBurguer">
        <!-- Campo de entrada para o nome do cliente -->
        <div class="input-container">
          <label for="nome">Nome do cliente:</label>
          <input
            type="text"
            name="nome"
            id="nome"
            v-model="nome"
            placeholder="Digite o seu nome"
            required
          />
        </div>
        <!-- Campo de seleção para o tipo de pão -->
        <div class="input-container">
          <label for="pao">Escolha o pão:</label>
          <select name="pao" id="pao" v-model="pao" required>
            <option value="">Selecione o seu pão</option>
            <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
              {{ pao.tipo }}
            </option>
          </select>
        </div>
        <!-- Campo de seleção para o tipo de carne -->
        <div class="input-container">
          <label for="carne">Escolha a carne do seu Hambúrguer:</label>
          <select name="carne" id="carne" v-model="carne" required>
            <option value="">Selecione o tipo de carne</option>
            <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
              {{ carne.tipo }}
            </option>
          </select>
        </div>
        <!-- Checkbox para selecionar os opcionais -->
        <div id="opcionais-container" class="input-container">
          <label id="opcionais-title" for="carne">Selecione os opcionais:</label>
          <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
            <input
              type="checkbox"
              name="opcionais"
              v-model="opcionais"
              :value="opcional.tipo"
            />
            <span>{{ opcional.tipo }}</span>
          </div>
        </div>
        <!-- Botão de envio -->
        <div class="input-container">
          <input
            class="submit-btn"
            type="submit"
            value="Criar meu Hambúrguer!"
          />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "BurgerForm",
  components: {
    Message,
  },
  data() {
    return {
      // Dados dos ingredientes
      paes: null,
      carnes: null,
      opcionaisdata: null,
      // Dados do formulário
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      // Mensagem de retorno após o envio do pedido
      msg: null,
    };
  },
  methods: {
    // Função para obter os ingredientes do servidor
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      // Atribui os dados dos ingredientes aos respectivos campos de dados
      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    // Função para criar um pedido de hambúrguer
    async createBurguer(e) {
      e.preventDefault();

      // Cria o objeto de dados do pedido
      const data = {
        nome: this.nome,
        carne: this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };
      const dataJson = JSON.stringify(data);

      // Envia o pedido para o servidor
      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();

      // Exibe a mensagem de confirmação com o número do pedido
      this.msg = `Pedido Nº ${res.id} realizado com sucesso!`;

      // Limpa o formulário após o envio do pedido
      setTimeout(() => {
        this.msg = "";
        this.nome = "";
        this.carne = "";
        this.pao = "";
        this.opcionais = [];
      }, 3000);
    },
  },
  mounted() {
    // Chamada para obter os ingredientes quando o componente é montado
    this.getIngredientes();
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 450px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #8b4513;
  padding: 5px 10px;
  border-left: 4px solid #d2691e;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #d2691e;
  color: #f5deb3;
  font-weight: bold;
  border: 2px solid #8b4513;
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #d2691e;
}
</style>