<template>
  <v-container>
    <h1>Cadastro de Usuarios</h1>
    <hr>
    <v-form v-model="valid">
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
              :rules="rule"
              outlined
            />
          </v-col>
        </v-row>
         <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.cpfcnpj"
              placeholder="Cpf/cnpj"
              label="Cpf/cnpj"
              :rules="rule"
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
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              placeholder="Telefone"
              label="Telefone"
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
      valid:null,
      usuario: {
        id: null,
        nome: null,
        cpfcnpj:null,
        email:null,
        telefone:null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  methods: {
  async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('Preencha todos os campos obrigatórios')
        }

        let usuario = {
          nome: this.usuario.nome,
          cpfcnpj: this.usuario.cpfcnpj,
          email: this.usuario.email,
          telefone: this.usuario.telefone
        };

        if (!this.usuario.id) {
          await this.$axios.$post('http://localhost:3333/usuarios', usuario);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/usuarios');
        }

        await this.$axios.$post(`http://localhost:3333/usuarios/${this.usuario.id}`, usuario);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/usuarios');
      } catch (error) {
        this.$toast.error("Ocorreu um erro ao cadastrar novo usuario");
      }
    },
    async getById (id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/categorias/${id}`);
    },
    pressionarTecla (evento) {
      console.log(evento)
      if (evento.keyCode === 13) {
        this.cadastrar()
      }
    },  
  }
}
</script>