<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h1>{{name}}</h1>
      <q-img :src="url" :ratio="1" width="250px" />
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Nome do pokemon" />
      <q-btn color="primary" label="Pesquisar" @click="getPokemon" />
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import api from "../services/api";

export default defineComponent({
  name: 'IndexPage',

  data () {
    return {
      name: "",
      url: "",
      search: "ivysaur",
    }; 
  },

  async beforeMount() {
    await this.getPokemon();
  },

  methods: {
    getPokemon() {
      api
        .get(`/pokemon/${this.search}/`)
        .then((response) => {
          this.name = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
          this.triggerPositive();
        })
        .catch((err) => {
          this.triggerNegative();
        });
    },
    triggerPositive () {
      this.$q.notify({
        type: 'positive',
        position: 'top',
        message: 'Pokemon Encontrado.'
      })
    },
    triggerNegative () {
      this.$q.notify({
        type: 'negative',
        position: 'top',
        message: 'Pokemon NÃO Encontrado.'
      })
    },
  },
});
</script>
