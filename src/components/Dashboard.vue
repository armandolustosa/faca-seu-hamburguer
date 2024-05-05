<template>
  <div id="burger-table">
    <!-- Componente de mensagem -->
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="burger-table-heading">
        <!-- Cabeçalho da tabela -->
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <!-- Linhas da tabela -->
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.nome }}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div>
          <!-- Lista de opcionais -->
          <ul v-for="(opcional, index) in burger.opcionais" :key="index">
            <li>{{ opcional }}</li>
          </ul>
        </div>
        <div>
          <!-- Seleção de status e botão de cancelar -->
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option
              :value="s.tipo"
              v-for="s in status"
              :key="s.id"
              :selected="burger.status == s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "Dashboard",
  components: {
    Message,
  },
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null,
    };
  },
  methods: {
    // Função para obter os pedidos
    async getPedidos() {
      const req = await fetch("http://localhost:3000/burgers");
      const data = await req.json();
      this.burgers = data;

      // Obtém os status dos pedidos
      this.getStatus();
    },
    // Função para obter os status dos pedidos
    async getStatus() {
      const req = await fetch("http://localhost:3000/status");
      const data = await req.json();
      this.status = data;
    },
    // Função para cancelar um pedido
    async deleteBurger(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE",
      });

      const res = await req.json();

      // Exibe mensagem de confirmação
      this.msg = `Pedido Nº ${res.id} removido com sucesso!`;

      setTimeout(() => {
        this.msg = "";
      }, 3000);

      // Atualiza a lista de pedidos
      this.getPedidos();
    },
    // Função para atualizar o status de um pedido
    async updateBurger(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({ status: option });
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();

      // Exibe mensagem de confirmação
      this.msg = `O pedido Nº ${res.id} foi atualizado para "${res.status}!"`;

      setTimeout(() => {
        this.msg = "";
      }, 3000);
    },
  },
  mounted() {
    // Carrega os pedidos quando o componente é montado
    this.getPedidos();
  },
};
</script>

<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burger-table-heading {
  font-weight: bold;
  padding: 10px;
  border-bottom: 3px solid #8b4513;
}

.burger-table-row {
  width: 100%;
  padding: 10px;
  border-bottom: 1px solid #d2691e;
}

#burger-table-heading div,
.burger-table-row div {
  flex: 1;
  min-width: 0;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  max-width: 5%;
  margin-right: 10px; 
}

select {
  padding: 4px 8px;
  margin-right: 10px;
  margin-bottom: 10px;
}

.delete-btn {
  background-color: #d2691e;
  color: #f5deb3;
  font-weight: bold;
  border: 2px solid #8b4513;
  padding: 4px 8px;
  font-size: 14px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #d2691e;
}
</style>
