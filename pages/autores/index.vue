<template>
  <v-container>
    <h1>Consulta de Autores</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getAutores"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="
            margin-left: -80%"
            outlined
            to="/autores/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="autores"
        :items-per-page="10"
        class="elevation-1"
      ><template v-slot:item.actions="{ item }">
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
  name: 'ConsultaAutoresPage',
  data () {
    return {
      headers: [
        {
          text: 'Código', //nome da coluna
          align: 'center', //alinhamento -centerp, end, start
          sortable: false, //se ermite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Nome',
          align: 'end',
          sortable: false,
          value: 'nome',
        },
        {
          text: 'Email',
          align: 'end',
          sortable: false,
          value: 'email',
        },
        { text: "", value:"actions" } 
      ],
      autores: []
    }
  },
  created () {
    this.getAutores()
  },
  methods: {
    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },
     async deleteItem (autor){
      try { 
         if(confirm(`Deseja deletar o autor id ${autor.id} - ${autor.nome}? `))
        {
        let response = await this.$axios.$post('http://localhost:3333/autores/deletar', { id: autor.id });
        this.$toast(response.message)
        this.getAutores();
        }
      } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
      }
    },
    async editItem (autor) {
      this.$router.push({
        name: 'autores-cadastro',
        params: { id: autor.id }
      });
    }
  }
}
</script>
 