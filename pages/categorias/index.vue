<template>
  <v-container>
    <h1>Consulta de Categorias</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getCategorias"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="
            margin-left: -80%"
            outlined
            to="/categorias/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="categorias"
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
  name: 'ConsultaCategoriasPage',
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
        { text: "", value: "actions" }
      ],
      categorias: []
    }
  },
  created () {
    this.getCategorias()
  },
  methods: {
    async getCategorias () {
    try {
        this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
    } },
    async deleteItem (categoria){
      try { 
         if(confirm(`Deseja deletar a categoria id ${categoria.id} - ${categoria.nome}? `))
        {
        let response = await this.$axios.$post('http://localhost:3333/categorias/deletar', { id: categoria.id });
        this.$toast(response.message)
        this.getCategorias();
        }
      } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
      }
      },
    async editItem (categoria) {
      this.$router.push({
        name: 'categorias-cadastro',
        params: { id: categoria.id }
      });
    }
   }
}
</script>
