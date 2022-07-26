<template>
  <v-container>
    <h1>Buscar Livro</h1>
    <hr>
    <v-autocomplete
      @change="pesquisar"
      style="
      margin-top: 3% "
      v-model="livro.id"
      :items="livros"
      outlined
      item-text="titulo"
      item-value="id">
    </v-autocomplete>
    <hr>
    <v-container v-if="emprestado">
      <h1>Indisponivel para empréstimo</h1>
      <br>
      <h3>Emprestado para o usuário:</h3> 
      <h4>id:{{usuario.id}}</h4>
      <h4>Nome: {{usuario.nome}}</h4>
      <h4>E-mail:{{usuario.email}} </h4>
      <h4>Telefone: {{usuario.telefone}}</h4>
      <br><br>
       <h4>Data para devolução: {{emprestimo.prazo}}</h4>
    </v-container>
    <v-container v-else>
      <h1>O livro está disponivel para empréstimo</h1>
      <br>
      <h3>Id: {{livro.id}}</h3>
      <h3>Título: {{livro.titulo}}</h3>
      <br>
      <h3>Sinopse:</h3>
      <h4>{{livro.sinopse}}</h4>
      <h3>Autor: {{autor.id}}</h3>
      <h4>{{autor.nome}}</h4>
    </v-container>
    
  </v-container>
</template>

<script>

export default {
  name: 'BuscarLivrosPage',
  data () {
   return{
    livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idCategoria: null,
        idAutor: null
      },
      livros:[],
      emprestado:null,
      emprestimo:{},
      usuario:{},
      autor:{}
   }
  }, 
  created(){
    this.getLivros();
  },
  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },
    pesquisar (event) {
      this.consultar()
    },
    async consultar(){
      let res = await this.$axios.$post(`http://localhost:3333/emprestimos/verificar-status`, {idLivro: this.livro.id})
      console.log(res);
      if(!res.disponivel){
      this.emprestado = true
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${res.emprestimo[0].id}`)
      this.usuario = await this.$axios.$get(`http://localhost:3333/usuarios/${this.emprestimo.idUsuario}`)   
      }else{
        this.emprestado = false
        res = await this.$axios.$get(`http://localhost:3333/livros/${this.livro.id}`)
        console.log(res);
        this.livro = res
        this.autor = await this.$axios.$get(`http://localhost:3333/autores/${res.idAutor}`)
      }

    }
  }
}
</script>