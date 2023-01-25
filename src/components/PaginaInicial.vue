<template>
    <div>
        <img :src="pokemon_image" alt="Pokemon" class="pokemon_image" />
        <h1 class="pokemon_data">
         <span class="pokemon_name">{{ name  }}</span>
        </h1>
        <form class="form" @submit.prevent="pesquisar()">
            <input type="search" class="input_search" placeholder="Nome ou Numero" name="input_search" v-model="input_search" required/>
        </form>
        <div class="buttons">
            <button class="button btn-prev" @click="prev()">Voltar</button>
            <button class="button btn-next" @click="next()">Proximo</button>
        </div>
        <img src="../assets/images/pokedex.png" alt="Pokedex" class="pokedex" />


   </div>
  </template>
  
<script>
import axios from 'axios'
import MissingNo from '@/assets/images/MissingNo.png'
 export default {
    name: 'PaginaInicial',
    data:()=>({
    pokemon:{},
    name:'Carregando',
    id:1,
    pokemon_image:'',
    input_search:'',
    n:1
    }),
    
    methods:{
        getPokemon(pok){
         axios.get(`https://pokeapi.co/api/v2/pokemon/${pok}`)
           .then((res) => {
            this.pokemon = res.data
         })
         .catch(() => {
            this.tratarErro()
         });
         
        },
        tratarErro(){
         this.name = 'Não encontrado'
         this.pokemon_image = MissingNo

        },

        mountPokemon(){
           this.name = this.pokemon.name
           this.id = this.pokemon.id 
           this.pokemon_image = this.pokemon['sprites']['versions']['generation-v']['black-white']['animated']['front_default']
           if(this.id>649){
            this.pokemon_image = this.pokemon['sprites']['versions']['generation-v']['black-white']['front_default']
           }
           if(this.id>898){
            this.pokemon_image = this.pokemon['sprites']['other']['official-artwork']['front_default']
           }
           this.n = this.id     
           
        },
        next(){
           if(this.n<1008){
            this.n = this.n+1
           }
           this.getPokemon(this.n)
        },
        prev(){ 
          if(this.n>1){
            this.n = this.n - 1
          }
          this.getPokemon(this.n)
        },
        
        pesquisar(){
            let search = this.input_search.toLocaleLowerCase()
            this.getPokemon(search)
            this.input_search = ''
        }
     },
     watch:{
      pokemon(){
         this.mountPokemon()
      }
     }, 
    mounted(){ 
      this.getPokemon(this.id) 
     }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
   *{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family: 'Times New Roman', Times, serif;
   }
   div{
    display: inline-block;
    padding: 15px;
    position: relative;
   }

   .pokedex{
    width:100%;
    max-width: 425px;
   }

   .pokemon_image{
    position: absolute;
    bottom:52%;
    left:35%;
    width:20%;
   }
   .pokemon_data{
    position: absolute;
    font-weight: 600;
    color:#aaa;
    top:54.5%;
    left:35%;
    font-size:clamp(8px,4vw,25px);
   }
   .pokemon_name{
    color:#3a444d;
    text-transform: capitalize;
   }
   .form{
    position: absolute;
    width: 50%;
    top:65%;
    left:23%;
   }
   .input_search{
    width: 100%;
    padding: 4%;
    outline: none;
    border: 2px solid #333;
    border-radius:5px;
    font-weight: 600;
    color:#3a444d;
    font-size:clamp(8px,5vw,1rem);
    box-shadow: -3px 4px 0 #888, -5px 7px 0 #333;
   }
   .buttons{
    position: absolute;
    bottom:10%;
    left:50%;
    width: 65%;
    transform:translate(-57%,0) ;
    display: flex;
    gap:20px;
   }
   .button{
    width: 50%;
    padding:4%;
    border:2px solid #000;
    font-size: clamp(8px,5vw,1rem);
    font-weight: 600;
    color:white;
    background-color:#444 ;
    box-shadow: -2px 3px 0 #222, -4px 6px 0 #000;
   }
   .button:active{
    box-shadow: inset -4px 4px 0 #222;
    font-size: 0.9rem;
   }
  </style>
  