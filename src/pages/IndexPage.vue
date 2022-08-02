<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h1>{{name}}</h1>
      <q-img :src="url" :ratio="1" width="250px" />
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Nome do pokemon" />
      <q-btn color="primary" label="Pesquisar" @click="getPokemon()" />
    </div>
    <div class="row justify-between absolute full-width container-arrows">
      <q-icon 
        name="far fa-solid fa-circle-arrow-left" 
        color="primary" 
        class="q-ml-sm cursor-pointer"
        size="50px" 
        @click="getPokemon(id-1)"
      >
        <q-tooltip>
          Anterior
        </q-tooltip>
      </q-icon>
      <q-icon 
        name="far fa-solid fa-circle-arrow-right" 
        color="primary" 
        class="q-mr-sm cursor-pointer"
        size="50px"
        @click="getPokemon(id+1)"
      >
        <q-tooltip>
          Próximo
        </q-tooltip>
      </q-icon>
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
      id: 2,
      search: "ivysaur",
    }; 
  },

  async beforeMount() {
    await this.getPokemon();
  },

  methods: {
    getPokemon(id) {
      console.log(id);
      api
        .get(id>0 ? `/pokemon/${id}/` : `/pokemon/${this.search}/`)
        .then((response) => {
          this.id = response.data.id;
          this.name = response.data.name;
          this.search = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
          //this.triggerPositive();
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

<style lang="scss">
.container-arrows {

}
</style>