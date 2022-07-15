<template>
  <div class="title">
    <ul>
    <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand class="ml-2">
          <img src = "src/assets/logo.png" />
          <b style="color:black">{{ appName }}</b>
        </b-navbar-brand>
    </b-navbar>
    </ul>

    <main>
      <b> Como é o frete que você precisa?</b>

      <hr style="width:350px;color:black" >

      <div class="destino">
        Destino

          <select name="cars" id="cars" style="width: 275px; font-size: medium;" >
            <option value="volvo">Volvo</option>
            <option value="saab">Saab</option>
            <option value="mercedes">Mercedes</option>
            <option value="audi">Audi</option>
          </select>        
      </div>

      <div class="peso">
        Peso
        
        <input type="number" placeholder="Insira aqui o peso da carga em Kg" style="width: 300px; font-size: medium;">
      </div>

      <div class="body-button">
        <button v-on:click="buttonAnalise" style="background-color: #2aad61 !important;">Analisar</button>
      </div>

      <div class="body-results">
        <b>Estas são as melhores alternativas de frete que encontramos para você.</b>

        <hr style="width:350px;color:black" >
      </div>

      <div class="reply-cheap">
        <p>Frete mais barato: <b>{{ cheaper }}</b></p> 
      </div>

      <div class="reply-fast">
        <p>Frete mais rápido: <b>{{ fastest }}</b></p> 
      </div>
    </main>
  </div>
  
</template>

<script>
import {
  BNavbar,
  BNavbarBrand,
} from 'bootstrap-vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
  },
  data() {
    const appName = ''
    const cheaper = ''
    const fastest = ''

    return {
      appName,
      cheaper,
      fastest
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina

    this.appName = 'MELHOR FRETE'
    this.cheaper = ''
    this.fastest = ''
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    methodFoo() {
      console.log(this.appName)
    },
    buttonAnalise(){
      this.cheaper = ''
      this.fastest = ''
    }
  },
}
</script>

<style scoped>
.title .navbar {
  background-color: #28aa78 !important;
  left: 0px;
}

.title .navbar-brand {
  font-size: x-large;
}

main{
  padding: 30px;
  font-size: larger;
}

ul {
  border: 1px solid black;
  background-color: #28aa78 !important;

}

.destino{
  margin-left: 10px;
}

.peso{
  margin-top: 5px;
  margin-left: 10px;
}

.body-button{
  margin-left: 100px;
  margin-top: 20px;
}

.body-results{
  margin-top: 30px;
  font-size: small;
}


</style>
