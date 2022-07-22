<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="autor.id"
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
              v-model="autor.nome"
              placeholder="Nome"
              label="Nome"
              @keypress="pressionarTecla"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="autor.email"
              placeholder="Email"
              label="E-mail"
              @keypress="pressionarTecla"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/autores"
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
  name: 'CadastroAutoresPage',
  data () {
    return {
      valid: null,
      autor: {
        id: null,
        nome: null,
        email:null,
      },
      rule: [
       v => !!v || 'Esse campo é obrigatório' 
      ]
    }
  },
   created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },
  methods: {
    async cadastrar () {
     try 
     { 
        if (!this.valid) {
            return this.$toast.warning('Preencha todos os campos obrigatórios')
        }
        let autor = {
          nome: this.autor.nome,
          email: this.autor.email
        };
          if (!this.autor.id) {
            await this.$axios.$post('http://localhost:3333/autores', autor);
            this.$toast.success('Cadastro realizado com sucesso!');
            return this.$router.push('/autores');
          }
            await this.$axios.$post(`http://localhost:3333/autores/${this.autor.id}`, autor);
            this.$toast.success('Cadastro atualizado com sucesso!');
            return this.$router.push('/autores');
        
     } 
     catch (error) 
     {
       this.$toast.error("Ocorreu um erro ao cadastrar novo autor");
     }
    },
    pressionarTecla (evento) {
      console.log(evento)
      if (evento.keyCode === 13) {
        this.cadastrar()
      }
    }, 
    async getById (id) {
      this.autor = await this.$axios.$get(`http://localhost:3333/autores/${id}`);
    },
  }
}
</script>