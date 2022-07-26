<template>
  <v-container>
    <h1>Cadastro de Livros</h1>
    <hr><br>
    <v-form v-model="valid">
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
          <v-col
            cols="3"
          >
            <v-autocomplete
              v-model="livro.idAutor"
              :items="autores"
              outlined
              label="Autor"
              
              item-text="nome"
              item-value="id"
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-col
            cols="3"
          >
            <v-autocomplete
              style="
              margin-left: -6%"
              v-model="livro.idCategoria"
              :items="categorias"
              outlined
              label="Categoria"
              item-text="nome"
              item-value="id"
            ></v-autocomplete>
          </v-col>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.titulo"
              placeholder="Título"
              label="Título"
              :rules="rule"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              v-model="livro.sinopse"
              placeholder="Sinopse"
              label="Sinopse"
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
      Salvar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      valid:null,
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idCategoria: null,
        idAutor: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ],
      categorias: [],
      autores: []
    }
  },
  created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
    this.getAutores();
    this.getCategorias();
  },
  methods: {
     async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('Preencha todos os campos obrigatórios')
        }
        let livro = {
          titulo: this.livro.titulo,
          sinopse: this.livro.sinopse,
          idCategoria: this.livro.idCategoria,
          idAutor: this.livro.idAutor
        };

        if (!this.livro.id) {
          await this.$axios.$post('http://localhost:3333/livros', livro);
            this.$toast.success('Cadastro realizado com sucesso!');
            return this.$router.push('/livros');
        }
         await this.$axios.$post(`http://localhost:3333/livros/${this.livro.id}`, livro);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/livros');
      } catch (error) {
        console.log("Ocorreu um erro ao cadastrar novo livro");
      }
    }, 
    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    },
    async getById (id) {
      this.livro = await this.$axios.$get(`http://localhost:3333/livros/${id}`);
    },
  }
}
</script>