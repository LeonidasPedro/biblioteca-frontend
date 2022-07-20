<template>
  <v-container>
    <h1>Cadastrar Livro</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="livro.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.titulo"
              placeholder="Titulo"
              label="titulo"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.sinopse"
              placeholder="Sinopse"
              label="sinopse"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.idCategoria"
              placeholder="Código da categoria"
              label="Id da categoria"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.idAutor"
              placeholder="Id do autor"
              label="Id do autor"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/livros"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="cadastrar"
    >
      Cadastrar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      livro: {
        id: null,
        titulo: null,
        sinopse:null,
        idCategoria:null,
        idAutor:null

      }
    }
  },
  created (){
    
  },
  methods: {
    async cadastrar () {
       let livro = {
        nome: this.livro.titulo
      };
      let response = await this.$axios.$post('http://localhost:3333/livros', livro);
      console.log(response);
      confirm('Cadastrar novo livro?') ? location.reload():location.href = 'http://localhost:3000/livros'

    },

  }
}
</script>