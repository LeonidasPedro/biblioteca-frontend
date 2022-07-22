<template>
  <v-container>
    <h1>Consulta de Usuarios</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getUsuarios"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="
            margin-left: -80%"
            outlined
            to="/usuarios/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="usuarios"
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
  name: 'ConsultaUsuariosPage',
  data () {
    return {
      headers: [
        {
          text: 'Código', //nome da coluna
          align: 'center', //alinhamento -centerp, center, start
          sortable: false, //se ermite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'nome',
        },
        {
          text: 'Email',
          align: 'center',
          sortable: false,
          value: 'email',
        },
          {
          text: 'Telefone',
          align: 'center',
          sortable: false,
          value: 'telefone',
        },
          {
          text: 'Cpf',
          align: 'center',
          sortable: false,
          value: 'cpfcnpj',
        },
        { text: "", value: "actions" }
      ],
      usuarios: []
    }
  },
  created () {
    this.getUsuarios()
  },
  methods: {
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
      
    },
    async deleteItem (usuario){
      try { 
         if(confirm(`Deseja deletar a usuario id ${usuario.id} - ${usuario.nome}? `))
        {
        let response = await this.$axios.$post('http://localhost:3333/usuarios/deletar', { id: usuario.id });
        this.$toast(response.message)
        this.getUsuarios();
        }
      } catch (error) {
        this.$toast.error("Ocorreu um erro!!!")
      }
      },
    async editItem (usuario){
      this.$router.push({
        name: 'usuarios-cadastro',
        params: { id: usuario.id }
      });
    }
  }
}
</script>
