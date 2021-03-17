<template>
  <div class="container">
    <br />
    <br />
    <div class="container">
      <div>
        <Titulo titulo="Lista de Compras" />
        <div class="row">
          <div class="col-11">
            <div class="form-floating">
              <input
                type="text"
                class="form-control"
                id="floatingInput"
                placeholder="Nome do item"
                v-model="nome"
                v-on:keyup.enter="addItem()"
              />
              <label for="floatingInput"> Adicionar item </label>
            </div>
          </div>
          <button
            type="button"
            @click="addItem()"
            class="btn btn-success col-1"
          >
            Adicionar
          </button>
        </div>
      </div>

      <br />
      <div class="container">
        <table class="table table-striped table-hover" v-if="lista.length">
          <thead class="">
            <th>Ordem</th>
            <th>Item</th>
            <th>Opções</th>
          </thead>
          <tbody>
            <tr scope="row" v-for="(item, index) in lista" :key="index">
              <td>{{ item.id }}</td>
              <!-- <td>{{ item.id }}</td> -->
              <td>{{ item.nome }}</td>
              <td>
                <button
                  type="button"
                  class="btn btn-danger"
                  @click="remover(item)"
                >
                  Remover
                </button>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="row">
          <div
            class="alert alert-warning col-11"
            role="alert"
            v-if="!lista.length"
          >
            <strong>Nenhum item cadastrado</strong> , adicione um novo item a
            sua lista de compras!
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Titulo from "../titulo/Titulo";

export default {
  components: {
    Titulo,
  },
  data() {
    return {
      nome: "",
      lista: [],
    };
  },
  created() {
    this.$http
      .get("http://localhost:3000/items")
      .then((res) => res.json())
      .then((items) => (this.lista = items));
  },
  props: {},
  methods: {
    addItem() {
      let _item = {
        nome: this.nome,
      };

      this.$http
        .post("http://localhost:3000/items", _item)
        .then((res) => res.json())
        .then((itemCadastrado) => {
          this.lista.push(itemCadastrado);
          this.nome = "";
        });
    },
    remover(item) {
      this.$http.delete(`http://localhost:3000/items/${item.id}`).then(() => {
        let indice = this.lista.indexOf(item);
        this.lista.splice(indice, 1);
      });
    },
  },
};
</script>

<!-- adicionando "scoped" o css fica limitado apenas para este componente -->
<style scoped></style>
