<template>
  <v-container>
    <h1>Cadastro de Categorias</h1>
    <hr>
    <br><br>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              style="
              margin-bottom: -25%;"
              v-model="categoria.id"
              placeholder="Código"
              label="Código"
              :rules="rule"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="categoria.nome"
              placeholder="Nome"
              label="Nome"
              @keypress="handleKeyPress"
              :rules="rule"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/categorias"
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
  name: 'CadastroCategoriasPage',
  data () {
    return {
      valid:null,
      categoria: {
        id: null,
        nome: null
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
        return this.$toast.warning('O formulário de cadastro não é válido!')}
      let categoria = {
        nome: this.categoria.nome
      };
      await this.$axios.$post('http://localhost:3333/categorias', categoria);
      this.$toast.success('Cadastro realizado com sucesso!');
      this.$router.push('/categorias');
      } catch (error) {
        console.log("Ocorreu um erro ao cadastrar nova categoria");
      }
    },
   handleKeyPress (evento) {
      console.log(evento)
      if (evento.keyCode === 13) {
        this.cadastrar()
      }
    },    
  }
}
</script>