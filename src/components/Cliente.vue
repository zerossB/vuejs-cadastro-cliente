<template>
  <div class="card">
    <div class="card-content">
      <p class="card-title">
        <b>{{ toUpperCase(cliente.nome) }} </b>
      </p>
      <p>
        <span class="tag is-black" v-if="cliente.isPremium">Premium</span>
      </p>
      <small> <b>UUID:</b>{{ generateUUIDV4 }} </small>
      <p><b>Email:</b> {{ cliente.email }}</p>
      <p v-if="showIdade == true"><b>Idade:</b> {{ cliente.idade }}</p>
      <p v-else>Este usuário escondeu a idade!</p>
    </div>
    <div class="card-footer">
      <button
        class="button is-danger is-light card-footer-item"
        v-if="cliente.isPremium"
        @click="changePremium(cliente)"
      >
        Remover Premium!
      </button>
      <button
        class="button is-black card-footer-item"
        v-else
        @click="changePremium(cliente)"
      >
        Tornar Premium!
      </button>
      <button
        class="button is-danger card-footer-item"
        @click="emitirEventoDelete"
      >
        Deletar Cliente
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Cliente",
  data() {
    return {};
  },
  props: {
    cliente: Object,
    showIdade: Boolean,
  },
  methods: {
    changePremium: (cliente) => {
      cliente.isPremium = !cliente.isPremium;
    },
    emitirEventoDelete: function () {
      console.log("Emitindo do Filho", this.cliente.nome);
      this.$emit("meDelete", {
        cliente: this.cliente,
        component: this,
      });
    },
    toUpperCase: function (value) {
      return value.toUpperCase();
    },
  },
  computed: {
    generateUUIDV4: function () {
      var current_date = new Date().getTime();
      var uuid = "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(
        /[xy]/g,
        (value) => {
          var result = (current_date + Math.random() * 16) % 16 | 0;
          current_date = Math.floor(current_date / 16);
          return (value == "x" ? result : (result & 0x3) | 0x8).toString(16);
        }
      );
      return uuid;
    },
  },
};
</script>

<style scoped>
.is-black {
  color: gold !important;
}
</style>
