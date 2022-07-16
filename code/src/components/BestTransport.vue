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
      valCity,
      
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina

    this.appName = 'MELHOR FRETE'

    axios.get('http://localhost:3000/transport')
      .then((resp)=>{
        this.data_frete = resp.data
        this.get_cities()
      })
  },
  
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    buttonAnalise(){
      var select_default = 'Selecione aqui o destino do frete'
      if( (this.valCity != select_default || this.valCity != '')  && 
          (this.valPeso > 0 )){
        var infos = []
        for(const data of this.data_frete){
          if(data.city == this.valCity){
            infos.push(data)
            //console.log(info)
          }
        }
        //console.log(infos)
        
        // selecting fastest and cheapes from infos list
        var fastest_index = 0
        var cheaper_index = 0
        for (let index = 0; index < infos.length; index++) {
          const distance = parseFloat(infos[index].lead_time.slice(0,infos[index].lead_time.length-1));
          const cost = this.valPeso <= 100 ?  parseFloat(infos[index].cost_transport_light.slice(2)) :
                                              parseFloat(infos[index].cost_transport_heavy.slice(2))
          
          if(distance < parseFloat(infos[fastest_index].lead_time.slice(0,infos[fastest_index].lead_time.length-1)) )
            fastest_index = index;
          
          if(cost < (this.valPeso <= 100 ? parseFloat(infos[cheaper_index].cost_transport_light.slice(2)) :
                                            parseFloat(infos[cheaper_index].cost_transport_heavy.slice(2))))
            cheaper_index = index;
        }
        const cost_total_cheaper = '' + (this.valPeso <= 100 ?  parseFloat(infos[cheaper_index].cost_transport_light.slice(2)) :
                                              parseFloat(infos[cheaper_index].cost_transport_heavy.slice(2))) * this.valPeso
        
        
        if(infos[fastest_index].lead_time === infos[cheaper_index].lead_time)
            fastest_index = cheaper_index
         
        const cost_total_fastest = '' + (this.valPeso <= 100 ?  parseFloat(infos[fastest_index].cost_transport_light.slice(2)) :
                                              parseFloat(infos[fastest_index].cost_transport_heavy.slice(2))) * this.valPeso
        this.cheaper = 'Transportadora ' + infos[cheaper_index].name + ' - R$ ' + (cost_total_cheaper)  + ' - ' + infos[cheaper_index].lead_time
        
        this.fastest = 'Transportador '  + infos[fastest_index].name + ' - R$ ' + (cost_total_fastest) + ' - ' + infos[fastest_index].lead_time

      }

      //console.log(this.valCity)
      //console.log(this.valPeso)
      
    },
    get_cities(){
      var duplicated_city = ' '
      this.valCity = 'Selecione aqui o destino do frete'
      var options = "<option>Selecione aqui o destino do frete</option>";
      for(const data of this.data_frete){

        if(!(duplicated_city.includes(data.city))){
          options += "<option>"+ data.city +"</option>";
          duplicated_city += data.city + " "
        }
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
