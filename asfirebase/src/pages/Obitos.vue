<template>
  
  <div class="container"> 
    <div class="main">
      <div class="my-auto">
        <div class="row">
            <div class="col-lg-4 col-md-8 col-12 mx-auto">
                <div class="mt-n4 mx-3 z-index-2">
                    <div class="bg-gradient-primary shadow-primary border-radius-lg py-3 pe-1">
                        <h4 class="text-white font-weight-bolder text-center mt-2 mb-0">Óbitos</h4>
                    </div>           
                </div>
            </div>
        </div>
      </div> 
   
    <br>
    <form id="formulario" class="row g-3">
      <div class="col">
 
        <select id="ubs" class="form-select" v-model="Responsavel">
            <option v-for="resp in users" :key="resp.id">{{resp.name}}</option>
        </select>    </div>

      <div class="col">
        <select id="ubs" class="form-select" v-model="Ubs">
            <option v-for="resp in users" :key="resp.id" selected>{{resp.ubs}}</option>
        </select>
   


      </div>
      <div class="row g-3">
        <div class="col">
          <label>Nome</label>
          <input class="form-control" type="text" placeholder="Nome" maxlength="150" v-model="nome">
        </div>
        <div class="col">
          <label>Nome da Mãe</label>
          <input class="form-control" type="text" placeholder="Nome da Mãe" maxlength="150" v-model="nomedamae">
        </div>
        <div class="col">
          <label>Endereço</label>
          <input class="form-control" type="text" placeholder="Endereço" maxlength="150" v-model="endereco">
        </div>

        <div class="col"> 
          <label>Municipio de Residência</label>
          <input class="form-control" type="text" placeholder="Municipio de Residência" maxlength="50" v-model="municipioderesidencia">
        </div>

        
      </div>
      <div class="row g-3">

        <div class="col">
          <label>Idade</label>
          <input class="form-control" type="number" placeholder="Idade" maxlength="2" v-model="idade">
        </div>

        <div class="col"> 
          <label>Nº do Prontuario</label>
          <input class="form-control" type="number" placeholder="Pronturario" maxlength="50" step="0.01" v-model="prontuario">
        </div>

        <div class="col"> 
          <label>Data do Óbito</label>
          <input class="form-control" type="date" placeholder="Data do Óbito" v-model="datadoobito">
        </div>
      </div>


      <div class="row g-3">
        <div class="col">  
          <label>Sexo</label>
          <select class="form-select" size="1" aria-label="size 1 select example" v-model="sexo">
              
              <option value="Masculino">Masculino</option>
              <option value="Feminino">Feminino</option>              
            </select>
        </div>
 
        <div class="col">  
          <label>Local do Óbito</label>
          <select class="form-select" size="1" aria-label="size 2 select example"  v-model="localdoobito">              
              <option selected value="Hospitalar">Hospitalar</option>
              <option value="Domiciliar">Domiciliar</option>              
            </select>
        </div>
      </div>
      <div class="row g-3">
        <div class="col">  
          <label>Causa Referida</label>
          <input class="form-control" type="text" placeholder="Causa" maxlength="2000"  v-model="Causa">
        

        </div>
      </div>
      <div class="row g-3">
          <div class="text-center">
            <button @click="cadastrarObitos" type="button" class="btn gradient-custom-2 w-40 my-4 mb-2 ">Cadastrar Obito</button>
          </div> 
          <div class="text-center">
            <router-link to="/editarobitos"><button  type="button" class="btn gradient-custom-2 w-40 my-4 mb-2 ">Editar Obito</button></router-link>
          </div>
          <div class="col-lg-4 col-md-8 col-12 mx-auto">
      <div class="text-center">
        <div v-if="error != undefined">
            <div class="notification is-danger">
                <p>{{ error }}</p>  
            </div>
            </div>
            <div v-if="deucerto != undefined">
              <div class="notification is-primary">
                  <p>{{ deucerto }}</p>  
              </div>
            </div>
      </div>
    </div>
         


        </div>
    </form>  
    <br>
  
            



  </div>
  </div>
  

  
</template>

<script>


import axios from 'axios';

export default {



  created(){
    var req = {
        headers:{
        Authorization: "Bearer " + localStorage.getItem('token')
        }
      }

    axios.get("http://localhost:8686/user",req).then(res => {
      console.log(res);
      this.users = res.data
    }).catch(err => {
      console.log(err);
    })
    console.log("usuarios em obitos");
    },


  data(){
      return{
          Ubs: '', // define a primeira opção do array como selecionada
          users: [],
          Responsavel:"",
          nome:"",
          nomedamae:"",
          idade:0,
          endereco:"",
          municipioderesidencia: "",
          sexo:"",
          prontuario:0,
          datadoobito:0,
          localdoobito:"",
          Causa:"",
          error: undefined,
          deucerto: undefined,

          
      }
  },
  methods:{


      
    cadastrarObitos(){
      axios.post("http://localhost:8686/obitos",
      
      
      {
        
          responsavel: this.Responsavel,
          ubs: this.Ubs,
          nome: this.nome,
          nomedamae: this.nomedamae,
          idade: this.idade,
          endereco: this.endereco,
          municipioderesidencia:this.municipioderesidencia,
          sexo: this.sexo, 
          prontuario: this.prontuario,
          datadoobito: this.datadoobito,
          localdoobito: this.localdoobito,
          Causa: this.Causa,

          
          
      }).then(res =>{
          
          console.log(res)
          var msgDeucerto = res.request.responseText;
          this.deucerto = msgDeucerto;
       
        }).catch(err => {
                if (err.response) {
                    var msgErro = err.response.data.err;
                    this.error = msgErro;
                }else{
                    console.log(err);
                    this.error = "Erro ao processar a solicitação";
                }
            })
     
  }

  },
  components(){
  
  }

}
</script>

<style scoped>
.gradient-custom-2 {
/* fallback for old browsers */
background: #fccb90;
color:#fff;

/* Chrome 10-25, Safari 5.1-6 */
background: -webkit-linear-gradient(195deg, #101cc2 0%, #D81B60 100%);
/* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
background: linear-gradient(195deg, #101cc2 0%, #D81B60 100%);
}
</style>

