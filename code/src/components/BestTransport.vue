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

    <main class="main-body">
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

      <div class="analise-button">
        <button class="analise-button-b" v-on:click="buttonAnalise">Analisar</button>
      </div>

      <div class="body-results" id="body-results-id">
        <b>Estas são as melhores alternativas de frete que encontramos para você.</b>

        <hr style="width:440px;color:black" >
     

        <div class="reply-cheap box">
          <img class="fast-logo" src="../assets/cheap-icon.png">
          <p class="reply-fast-p">Frete mais barato: <b>{{ cheaper }}</b></p> 
        </div>

        <div class="reply-fast box">
          <img class="fast-logo" src = "../assets/fast-icon.png"/>
          <p class="reply-fast-p"> Frete mais rápido: <b>{{ fastest }}</b></p> 
        </div>
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
    const valPeso = ''
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
      if( (this.valCity != select_default && this.valCity != '')  && 
          (this.valPeso > 0 )){
        document.getElementById("body-results-id").style.display = "inline";
        

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
        var cost_total_cheaper =  (this.valPeso <= 100 ?  parseFloat(infos[cheaper_index].cost_transport_light.slice(2)) :
                                              (parseFloat(infos[cheaper_index].cost_transport_heavy.slice(2)))) * this.valPeso
        
        cost_total_cheaper = cost_total_cheaper.toFixed(2)
        
        if(infos[fastest_index].lead_time === infos[cheaper_index].lead_time)
            fastest_index = cheaper_index
        
        var cost_total_fastest = (this.valPeso <= 100 ?  parseFloat(infos[fastest_index].cost_transport_light.slice(2)) :
                                              (parseFloat(infos[fastest_index].cost_transport_heavy.slice(2)))) * this.valPeso
        
        cost_total_fastest = cost_total_fastest.toFixed(2)

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
.main-body{
  display: flex;
  align-items: center;
  align-content: center;
  flex-direction: column;
}

ul {
  border: 1px solid black;
  background-color: #28aa78 !important;

}

.destino{
  margin-left: 1vw;
}

.peso{
  margin-top: 5px;
  margin-left: 1vw;
}

.analise-button{
  margin-top: 20px;
  
}
.analise-button-b{
  background-color: rgba(89, 190, 131, 0.7) !important;
  border-radius: 6px !important;
}


.body-results{
  margin-top: 30px;
  font-size: small;
  display: none;
}

.body-results p{
  /* outline-style: dashed;
  outline-width: 2px;
  margin-top: 15px !important;
  padding: 5px;
  border-radius: 8px; */
}

.box{
  outline-style: dashed;
  outline-width: 2px;
  margin-top: 15px !important;
  padding: 5px;
  border-radius: 8px;
  display: flex;
  gap: 5px;
}

.reply-cheap{
  /* width: fit-content; */
  border-radius: 8px;
  background-color: rgba(40, 170, 120, 0.664);

}

.reply-fast{
  /* width: fit-content; */
  border-radius: 8px;
  background-color: rgba(45, 182, 236, 0.726);
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


.reply-fast-p{
  margin: 0;
}

.fast-logo{
  height: 20px;
  margin: 0;
}
</style>
