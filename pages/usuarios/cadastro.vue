<template>
  <v-container>
    <h1>Cadastro de usuarioes</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="usuario.id"
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
              v-model="usuario.nome"
              placeholder="Nome"
              label="Nome"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.email"
              placeholder="Email"
              label="E-mail"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/usuarios"
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
  name: 'CadastroUsuariosPage',
  data () {
    return {
      usuario: {
        id: null,
        nome: null,
        email:null
      }
    }
  },
  methods: {
    async cadastrar () {
      let usuario = {
        nome: this.usuario.nome
      };
      let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
      console.log(response);
      confirm('Cadastrar novo usuario?') ? location.reload():location.href = 'http://localhost:3000/usuarios'

    }
  }
}
</script>