<template>
    <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-container">
    
              <div class="modal-top">
    
                <button class="modal-default-button-left" @click="$emit('close')"><BIconArrowLeft color="#c7c6f9" style="width: 20px; height: 20px;"/></button>
                <slot name="header">
                 Informe seus Dados Endereço
                </slot>
                <button class="modal-default-button" @click="$emit('click')">
                   <BIconX color="#c7c6f9" style="width: 25px; height: 25px;"/>
                  </button>
              </div>
    
              <div class="modal-body">
                <!-- <slot name="body">
                  default body
                </slot> -->
                <form>
                    <div class=" justify-content-start mb-2  ">
                    <label  class="d-block"   >CEP *</label>
                    <input type="text" placeholder="Digite seu cep" maxlength="8"  v-model="address.CEP">
                </div>
                <div class="mb-2">
                    <label class="d-block" >Rua *</label>
                    <input type="text" placeholder="exemplo@exemplo" v-model="address.rua">
                </div>
                <div class="mb-2">
                    <label class="d-block" >Bairro *</label>
                    <input type="text" placeholder="bairro" v-model="address.bairro" >
                </div>
                <div class="mb-4">
                    <label class="d-block" >Cidade *</label>
                    <input type="text" placeholder="(xx) x xxxx-xxxx " v-model="address.localidade">
                </div>
    
                <div class="">
                <slot name="footer">
                    <button class="buttonNext mb-2" @click.prevent="getAdress">
                        Consulta cep
                  </button>
                    <button class="buttonNext"  @click.prevent="$emit('open')">
                    Próximo
                  </button>
                  
                 
                </slot>
              </div>
                </form>
                
            
              </div>
    
              
            </div>
          </div>
        </div>
      </transition>    
    </template>
    
    <script>
    
    import { BIconArrowLeft , BIcon , BIconX, AlertPlugin } from 'bootstrap-vue'
    import  axios from 'axios'
    
    export default {
    
        components: {
        BIcon,
        BIconArrowLeft,
        BIconX
      },
        data(){
    
            return{

                address:{
                    CEP: '',
                    rua: '',
                    bairro: '',
                    localidade: '',
                    uf: '',
                    complemento: '',
                },

                response: null,
                baseUrl: 'https://viacep.com.br/ws/',

                  
                   
                   showModal: false
    
    
            }
        },

        // computed: {
        // getAdress: function () {

        //     const addressCep = this.address.CEP
        //     axios.get(`'https://viacep.com.br/ws/${addressCep}/json/'`).then(
        //             response => console.log(response)
        //             )}
        // },

        watch:{
            CEP: function (novoCep, velhoCep){
                if(novoCep.length === 8 ) this.getAdress()
                else this.response = null
            }
        },
        methods:{
            submit(){
    
                localStorage.setItem(this.user,JSON.stringify(this.user));
                
                
            },

           async getAdress(){

               const url = `${this.baseUrl}${this.address.CEP}/json/`

             await  axios.get(url).then(resp =>{

                const data = resp.data
                if(!data.erro){
                    this.address.CEP = data.cep
                    this.address.rua = data.logradouro
                    this.address.bairro = data.bairro
                    this.address.localidade = data.localidade
                    this.address.uf = data.uf
                    this.address.complemento = data.complemento
                    
                } else {
                    Alert('Cep não encontrado')
                }
               }).catch(error =>{
                console.log(error)
               })
        
                

            

            
            }
    
        },
        

    }
    </script>
    
    <style scoped>
    .modal-mask {
      position: fixed;
      z-index: 9998;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      display: table;
      transition: opacity 0.3s ease;
    }
    
    .modal-wrapper {
      display: table-cell;
      vertical-align: middle;
    }
    
    .modal-container {
      width: 400px;
      height: 430px;
      margin: 0px auto;
      padding: 20px 30px;
      background-color: #1c294f;
      border-radius: 2px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
      transition: all 0.3s ease;
      
    }
    
    .modal-top {
      margin-top: 0;
      color: white;
      font-weight: bold;
    }
    
    .modal-body {
      margin: 20px 0;
    }
    
    .modal-default-button {
      float: right;
      border: 0;
      background: #1c294f;
    }
    
    
    .modal-default-button-left {
      float: left;
      border: 0;
      background: #1c294f;
    }
    
    /*
     * The following styles are auto-applied to elements with
     * transition="modal" when their visibility is toggled
     * by Vue.js.
     *
     * You can easily play with the modal transition by editing
     * these styles.
     */
    
    .modal-enter {
      opacity: 0;
    }
    
    .modal-leave-active {
      opacity: 0;
    }
    
    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
      -webkit-transform: scale(1.1);
      transform: scale(1.1);
    }
    
    input{
        background-color: #2a3555;
        border-radius: 5px;
        color: white;
        border:0;
        width: 100%;
    
    }
    
    .buttonNext{
       
      
      width: 100%;
      border: 0;
      border-radius: 5px;
      background: var(--bluePurple);
      color: var(--white);
      
    
    
    }
    </style>