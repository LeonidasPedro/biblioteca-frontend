<template>
  <v-container>
    <h1>Buscar Livro</h1>
    <hr>
    <v-autocomplete
      style="
      margin-top: 3% "
      v-model="livro.id"
      :items="livros"
      outlined
      label="livros"
      item-text="titulo"
      item-value="id">
    </v-autocomplete>
  </v-container>
</template>

<script>

export default {
  name: 'BuscarLivrosPage',
  data () {
   return{
    livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idCategoria: null,
        idAutor: null
      },
      livros:[]
   }
  }, 
  created(){
    this.getLivros();
  },
  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },

  
  }
}
</script>