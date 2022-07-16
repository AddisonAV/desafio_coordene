<template>
  <div class="title">
    <ul>
    <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand class="ml-2">
          <img class="logo" src = "../assets/logo.png" />
          <b style="color:black">{{ appName }}</b>
        </b-navbar-brand>
    </b-navbar>
    </ul>

    <main>
      <b> Como é o frete que você precisa?</b>

      <hr style="width:350px;color:black" >

      <div class="destino">
        Destino
        <div>
          <select class="select-cities" v-model="valCity" id="sel_cities" name="sel_cities"></select>
        </div>
        
                 
      </div>

      <div class="peso">
        Peso
        
        <div>
          <input 
            type="number" 
            placeholder="Insira aqui o peso da carga em Kg" 
            v-model="valPeso"
          >
        </div>
        
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
import axios from 'axios'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
  },
  data() {
    const appName = ''
    const cheaper = ''
    const fastest = ''
    const valPeso = 0
    const valCity = ''

    return {
      appName,
      cheaper,
      fastest,
      data_frete:undefined,
      valPeso,
      valCity
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina

    this.appName = 'MELHOR FRETE'
    this.cheaper = ''
    this.fastest = ''

    axios.get('http://localhost:3000/transport')
      .then((resp)=>{
        this.data_frete = resp.data
        this.get_cities()
      })
  },
  
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    buttonAnalise(){
      console.log(this.valCity)
      console.log(this.valPeso)
      
    },
    get_cities(){
      var options = "<option>Selecione aqui o destino do frete</option>";
      for(const data of this.data_frete){
        options += "<option>"+ data.city +"</option>";
      }
      document.getElementById("sel_cities").innerHTML = options;
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

.select-cities{
  font-size: small;
  height: 30px;
  background-color: rgba(36, 199, 240, 0.55);
  width: 300px;
}

input{
  width: 300px;
  height: 30px;
  font-size: small;
  background-color: rgba(36, 199, 240, 0.55);
  font-weight: 600;
}

.logo{
  width: 40px;
  margin-right: 10px;
}
</style>
