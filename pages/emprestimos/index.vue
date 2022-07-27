<template>
  <v-container>
    <h1>Buscar Emprestimo</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
             to="/consultarLivro"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="
            margin-left: -79%"
            outlined
            to="/emprestimos/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="emprestimos"
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
          text: 'Prazo',
          align: 'center',
          sortable: false,
          value: 'prazo',
        },
        {
          text: 'Usuario ID',
          align: 'center',
          sortable: false,
          value: 'idUsuario',
        },
         {
          text: 'Livros',
          align: 'center',
          sortable: false,
          value: 'livros[0].titulo',
        },
         {
          text: 'Data da Devolucao',
          align: 'center',
          sortable: false,
          value: 'devolucao',
        },


        { text: "", value: "actions" }
        
      ],
      emprestimos: []
    }
  },
  created () {
    this.getEmprestimos()
  },
  methods: {
    async getEmprestimos () {
     try {
       this.emprestimos = await this.$axios.$get('http://localhost:3333/emprestimos/');
       console.log(this.emprestimos);
   
     } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
     } },

     async editItem (emprestimo) {
        this.$router.push({
          name: 'emprestimos-cadastro',
          params: { id: emprestimo.id }
        })},

    async deleteItem (emprestimo){
      try { 
         if(confirm(`Deseja deletar o emprestimo id ${emprestimo.id}? `))
        {
        let response = await this.$axios.$post('http://localhost:3333/emprestimos/deletar', { id: emprestimo.id });
        this.$toast(response.message)
        this.getEmprestimos();
        }
      } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
      }
    },
      
  }
}
</script>
