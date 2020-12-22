<template>
  <div class="container">
    <div>
      <h1>Formulário de Cliente</h1>
      <hr />
      <div class="field">
        <label class="label">Nome</label>
        <div class="control">
          <input
            class="input"
            :class="{ 'is-danger': error.nome }"
            type="text"
            placeholder="Nome"
            v-model="form.nome"
          />
        </div>
        <p class="help is-danger" v-if="error.nome">
          O nome não pode ser vazio
        </p>
      </div>
      <div class="field">
        <label class="label">Email</label>
        <div class="control">
          <input
            class="input"
            :class="{ 'is-danger': error.email }"
            type="email"
            placeholder="Email"
            v-model="form.email"
          />
        </div>
        <p class="help is-danger" v-if="error.email">
          O email não pode ser vazio
        </p>
      </div>

      <div class="field">
        <label class="label">Idade</label>
        <div class="control">
          <input
            class="input"
            :class="{ 'is-danger': error.idade }"
            type="Idade"
            placeholder="Idade"
            v-model="form.idade"
          />
        </div>
        <p class="help is-danger" v-if="error.idade">
          A Idade tem que ser maior que 0
        </p>
      </div>
      <div class="field">
        <div class="control">
          <label class="checkbox">
            <input type="checkbox" v-model="form.isPremium" />
            Cliente Premium?
          </label>
        </div>
      </div>
      <button @click="cadastrarCliente" class="button is-primary">Cadastrar Cliente</button>
    </div>
    <hr />
    <div class="columns is-multiline">
      <div
        v-for="(cliente, index) in orderClientes"
        :key="index"
        class="column is-4"
      >
        <Cliente
          :cliente="cliente"
          :showIdade="true"
          @meDelete="deletarCliente($event)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
import Cliente from "./components/Cliente";

export default {
  name: "App",
  components: {
    Cliente,
  },
  data() {
    return {
      error: {
        nome: false,
        email: false,
        idade: false,
      },
      form: {
        nome: "",
        email: "",
        idade: 0,
        isPremium: false,
      },
      clientes: [
        {
          nome: "Guilherme Haynes",
          email: "guilherme.haynes@gmail.com",
          idade: 23,
          isPremium: true,
        },
        {
          nome: "Felipe Cabrera",
          email: "felipe.cabrera@gmail.com",
          idade: 25,
          isPremium: false,
        },
        {
          nome: "Tulio Camargo",
          email: "tulio.camargo@gmail.com",
          idade: 33,
          isPremium: true,
        },

        {
          nome: "Ariadne Bachiega",
          email: "ariadine.bachiega@gmail.com",
          idade: 33,
          isPremium: true,
        },
      ],
    };
  },
  methods: {
    cadastrarCliente: function () {
      this.error.nome = false;
      this.error.email = false;
      this.error.idade = false;

      if (this.form.nome.length == 0) {
        this.error.nome = true;
      } else if (this.form.email.length == 0) {
        this.error.email = true;
      } else if (this.form.idade == 0) {
        this.error.idade = true;
      } else {
        this.clientes.push({
          nome: this.form.nome,
          email: this.form.email,
          idade: this.form.idade,
          isPremium: this.form.isPremium,
        });

        this.form = {
          nome: "",
          email: "",
          idade: 0,
          isPremium: false,
        };
      }
    },
    deletarCliente: function ($event) {
      var email = $event.cliente.email;
      var novoArray = this.clientes.filter((cliente) => cliente.email != email);
      this.clientes = novoArray;
    },
  },
  computed: {
    orderClientes: function () {
      return _.orderBy(this.clientes, ["nome"], ["asc"]);
    },
  },
};
</script>

<style>
:root {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

input {
  margin: 5px 0px;
  padding: 5px 5px;
}

.error {
  color: red;
  font-weight: 800;
}
</style>;
