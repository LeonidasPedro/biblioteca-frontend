<template>
  <v-container>
    <h1>Buscar Livro</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getLivros"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="
            margin-left: -79%"
            outlined
            to="/livros/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="livros"
        :items-per-page="10"
        class="elevation-1"
      >
      <template v-slot:item.actions="{ item }">
          <v-icon
            small
            class="mr-2"
            @click="editItem(item)"
          >
          mdi-pencil
          </v-icon>
          <v-icon
            small
            @click="deleteItem(item)"
          >
          mdi-delete
          </v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn
          color="primary"
          @click="initialize"
            >
            Reset
          </v-btn>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaLivrosPage',
  data () {
    return {
      headers: [
        {
          text: 'ID', //nome da coluna
          align: 'center', //alinhamento -centerp, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Título',
          align: 'center',
          sortable: false,
          value: 'titulo',
        },
        {
          text: 'Autor',
          align: 'center',
          sortable: false,
          value: 'autor.nome',
        },
        {
          text: 'Categoria',
          align: 'center',
          sortable: false,
          value: 'categoria.nome',
        },
        { text: "", value: "actions" }
        
      ],
      livros: []
    }
  },
  created () {
    this.getLivros()
  },
  methods: {
    async getLivros () {
     try {
       this.livros = await this.$axios.$get('http://localhost:3333/livros/');
   
     } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
     } },
    async deleteItem (livro){
      try { 
         if(confirm(`Deseja deletar a livro id ${livro.id} - ${livro.titulo}? `))
        {
        let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: livro.id });
        this.$toast(response.message)
        this.getLivros();
        }
      } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
      }
      },
    async editItem (livro) {
        this.$router.push({
          name: 'livros-cadastro',
          params: { id: livro.id }
        });
      }
  }
}
</script>
