<template>

  <h1>{{ nombre }}</h1>
  <input v-model="nombre">
  <p v-if="nombre === 'JUAN'">JAJAJAJJA te llamas {{ nombre }}</p>
  <input type="text" v-model="nuevoNombre" placeholder="agregar nombre">
  <button @click="agregarNombree"> Agregar  </button>
  
  <input type="text" v-model="nombreEditado" placeholder="nombre">
  <ul>
    <li v-for="(array,index) in array" :key="index">
      
        {{ array }}<button @click="editar(index)" >Editar</button ><button @click="borrar(index)" > Borrar</button>
    </li>
  </ul>

  </template>


<script>

export default {
  name: "MiApp",
  data(){
    return{
        nombre:"kike".toUpperCase(),
        array:['kike','pedro','juan'],
        nuevoNombre:"",
        nombreEditado:""
    }
  },
  methods:{
    agregarNombree(){
        this.array.push(this.nuevoNombre)
    },
    borrar(index){
      this.array.splice(index,1)
    },
    editar(index){
        this.array.splice(index,1,this.nombreEditado)
    }

  }
}





</script>