<template>
  <v-container>
    <v-text-field
    style="width: 300px;"
    solo
    v-model="tela"
    ></v-text-field>
    <v-row>
      <v-btn
      class="mx-2"
      fab
      dark
      small
      @click="pressBotao('7')"
      color="primary"
      >
      <v-icon dark>
          7
      </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('8')"
      color="primary"
      >
      <v-icon dark>
       8
      </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('9')"
      color="primary"
      >
      <v-icon dark>
          9
      </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('-')"
      color="primary"
      >
      <v-icon dark>
          mdi-minus
      </v-icon>
      </v-btn>
    </v-row>
    <v-row>
        <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('4')"
      color="primary"
      >
      <v-icon dark>
          4
      </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('5')"
      color="primary"
      >
      <v-icon dark>
          5
      </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('6')"
      color="primary"
      >
      <v-icon dark>
          6
      </v-icon>
      </v-btn>
        <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('+')"
      color="primary"
      >
      <v-icon dark>
          mdi-plus
      </v-icon>
      </v-btn>
    </v-row>
    <v-row>
        <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('1')"
      color="primary"
      >
        <v-icon dark>
          1
        </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('2')"
      color="primary"
      >
        <v-icon dark>
          2
        </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('3')"
      color="primary"
      >
        <v-icon dark>
          3
        </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('*')"
      color="primary"
      >
        <v-icon dark>
          x
        </v-icon>
      </v-btn>
    </v-row>
    <v-row>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('0')"
      color="primary"
      >
      <v-icon dark>
        0
      </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('/')"
      color="primary"
      >
      <v-icon dark>
        /
      </v-icon>
      </v-btn>
      <v-btn
      class="mx-2"
      fab
      dark
      small
       @click="pressBotao('=')"
      color="primary"
      >
      <v-icon dark>
        =
      </v-icon>
      </v-btn>
    </v-row>
  </v-container>
</template>

<script>
export default{
  data(){
    return {
      tela:[],
      numero:[],
      numeros:['1','2','3','4','5','6','7','8','9','0'],
      operadores:['*', '/', '+', '=', '-'],
      operacao:[],
      num1: null,
      num2: null
    }
  },


  methods:{
    pressBotao(params){
      if(this.numeros.includes(params)){
          let numero = this.numero.push(params);
          console.log(numero);
          this.tela  = this.numero.join('')
          this.num1 ? this.num2 = this.numero : this.num1 = this.numero;
          console.log(this.num1);
          console.log(this.num2);
      }else{
          let operacao = this.operacao;
          this.num2.join('');
          num1.join('');

          this.operacao = params
          console.log(this.operacao);
       switch(operacao){
        case '+':
         Number.this.num1 += Number.this.num2;
          console.log(num1);
          break;
        case '-':
         Number.this.num1 -= Number.this.num2;
          break;
        case '*':
          Number.this.num1 *= Number.this.num2;
          break;
        case '/':
          Number.this.num1 /= Number.this.num2;
          break;
        }
      }
    }
  },
  
}
  
</script>

<style>
  .mx-2 { 
    margin-top: 10px;
  }
  
</style>