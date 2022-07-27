<template>
  <v-container>
    <h1>Cadastro de Emprestimos</h1>
    <hr><br>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="emprestimo.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
              :disabled="desabilitar" 
            />
          </v-col>
        </v-row>
         <v-row>
          <v-col cols="12">
            <v-autocomplete
              v-model="emprestimo.idUsuario"
              :items="usuarios"
              outlined
              label="Usuario"
              item-text="nome"
              item-value="id"
              :disabled="desabilitar" 
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-row>
         <v-col cols="12">
          <v-autocomplete
            v-model="emprestimo.livros"
            :items="livros"
            outlined
            dense
            chips
            small-chips
            label="livros"
            multiple
            :disabled="desabilitar" 
            item-text="titulo"
            item-value="id"
            :rules="rule"
          ></v-autocomplete>
        </v-col>
        </v-row>
        <v-row>
          <template>
            <v-simple-table 
            dark
            >
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">
                      titulo
                    </th>
                    <th class="text-left">
                      id
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="livro in mochila"
                    :key="livro.id"
                  >
                    <td>{{ livro.titulo }}</td>
                    <td>{{ livro.id }}</td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
          </template>
        </v-row>
         <v-row>
          <v-col>
            <h3>
              Prazo
            </h3>
            <v-date-picker 
            :disabled="desabilitar" 
            v-model="emprestimo.prazo"></v-date-picker>
          </v-col>
          <v-col>
            <h3>
              Data Devolução
            </h3>
            <v-date-picker 
            disabled 
            v-model="emprestimo.devolucao"></v-date-picker>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/emprestimos"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="cadastrar"
    >
      Salvar
    </v-btn>
    <v-btn
      outlined
      @click="popularMochila"
    >
      Salvar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroEmprestimosPage',
  data () {
    return {
      valid:null,
      emprestimo: {
        	id: null,
	        prazo: null,
	        devolucao: null,
	        idUsuario: null,
          livros:[]
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ],
      categorias: [],
      usuarios: [],
      livros: [],
      mochila:[],
      desabilitar:false
    }
  },
  created () {
    if (this.$route?.params?.id) {
      this.desabilitar = true
      this.getById(this.$route.params.id)
    }
    this.getUsuarios();
    this.getLivros();
  },
  methods: {
     async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('Preencha todos os campos obrigatórios')
        }
        let emprestimo = {
          id: this.emprestimo.id,
          prazo: this.emprestimo.prazo,
          devolucao: this.emprestimo.devolucao,
          idUsuario: this.emprestimo.idUsuario,
          livros: this.emprestimo.livros,
        };

        if (!this.emprestimo.id) {
          await this.$axios.$post('http://localhost:3333/emprestimos', emprestimo);
            this.$toast.success('Cadastro realizado com sucesso!');
            return this.$router.push('/emprestimos');
        }
         await this.$axios.$post(`http://localhost:3333/emprestimos/${this.emprestimo.id}`, emprestimo);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/emprestimos');
      } catch (error) {
        this.$toast.error("Ocorreu um erro ao cadastrar novo emprestimo");
      }
    }, 
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
    },
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },
     async getById(id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${id}`);
    },
    async popularMochila(){
      console.log(this.emprestimo.livros); 
      console.log(this.livros)
    }

  }
}
</script>