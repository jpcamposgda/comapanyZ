<template>

<div class="containerAll">
  

<div class="containerDiv">
  <button class="containerPlano" v-for="item in Mydb.data.activePlans" :key="item.id" @click="handleFind(item)">
    {{ item.name }}
  </button>
</div>

<div class="d-flex ">
<div v-if="GetItem" class="caixaPlano ">
  <span class="planTitle" >O {{ GetItem }} irá incluir</span>
  <div style=" border-bottom: 1px solid #424242;"  class="d-flex justify-content-between mt-2 " v-for="(cont, index) in conteudo[0]" :key="index"  >
    
    <span>{{cont.service}}</span>

    <span> {{ cont.baseQuantity }}</span>
  </div>
  <div class="d-flex justify-content-between"> 
    <span> Aumentar quantidade de dominios</span>
    <input  type="range" min="1" max="70" step="1" v-model="addOnPriceAmt">
  </div>
</div>


<div v-if="GetItem" class="caixaPlano ml-4">
<span class="planTitle" >Sua Escolha</span>
<div style=" border-top: 1px solid #424242;" class=" d-flex justify-content-between mt-2">
  <span > {{ GetItem }} </span>
  <span> R$ {{ planBaseAmt }} </span>
</div>
<div v-if="addOnPriceAmt" style=" border-top: 1px solid #424242;" class=" d-flex justify-content-between mt-2">
  <span class="">+<input class="inputRang" type="number" disabled  v-model="addOnPriceAmt"></span>
  <span > R$ {{ total }} </span>
</div>

<div style=" border-top: 1px solid #424242;" class=" d-flex justify-content-between mt-2">
  <span>TOTAL</span>
  
  <span>R$ {{ totalPlan }} </span>
</div>
</div>
</div>




</div>


</template>

<script>


import db from "../db.json"
import resDb from "../responseDb.json"
export default {
  name: 'IndexPage',

data(){

  return{

    Mydb: db,
    MyResponse: resDb,
    GetItem: "",
    service: "",
    conteudo:[],
    planBaseAmt: 0,
    addOnPriceAmt: 0
    
  }
},

computed: {
      total: function () {
      return this.addOnPriceAmt * 5
    },

    totalPlan: function () {

     const addPrice =  Number(this.planBaseAmt)
     
     const all = Number(this.total)

     return all + addPrice

    //  const basePrice =  Number(this.planBaseAmt)
      
    //  const total = addPrice + basePrice
    //  return total.toFixed(2)
     
     
  },

},

methods:{

  handleFind(item){

    this.conteudo.length = 0;
    
    const MyRes =  this.MyResponse.data.planInfo.find(i => i.id == item.id)

    if(MyRes){

      this.GetItem = item.name

      this.planBaseAmt = MyRes.planBaseAmt

      
      
      this.conteudo.push(MyRes.contents)
      

      

   
      
    }

    
  
    
    
    


    
    

  }


  }







}
</script>

<style>
:root {
  --white: #FFF;
  --black: #000;
  --dark-900: #101026;
  --dark-700: #1D1D2E;
  --bluePurple: #625DF5;
}

body{
  background: var(--dark-900);
}

div{

  color: var(--white)
}

.containerAll{

display: block;
}
.containerDiv{

display: flex;
margin-top: 2rem;
}

.containerPlano{

width: 300px;
margin: 2px;
text-align: center;
background-color: var(--dark-900);
border: 1px solid var(--white);
border-radius: 10px;


padding: 0.4rem;
color: var(--white);

}


.containerPlano:focus{

border: 1px solid var(--bluePurple);
color: var(--bluePurple)
}

.detailPlan{

margin-right: 100px;
padding: 100px;
}

.containerForm{

margin-left: 100px;
}

.planTitle{

  font-weight: bold;
  color: white;
  
  
}

.caixaPlano{
  width: 40%;
  margin-top: 12px;
  border: 1px solid #424242;
  border-radius: 10px;

}


.inputRang{
  background: #101026;
  border: 0;
  color: var(--white);
}

input[type=number]::-webkit-inner-spin-button { 
  -webkit-appearance: none;
  
}
input[type=number] { 
 -moz-appearance: textfield;
 appearance: textfield;

}
</style>