<template>

  <div v-if="clientes.length <= 0">
    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
      Crear
    </button>
  </div>

  <ul style="align-items: center; justify-content: center; text-align: center; display: flex; flex-direction: column;">
    <li v-for="(cliente, index) in clientes" :key="index">



      <div class="card">
        <header class="card-header">
          <p>{{ cliente.fecha }}</p>
          <span class="title">User {{ index }}</span>
        </header>
        <div class="card-author">
          <a class="author-avatar" href="#">
            <span>
            </span></a>
          <svg class="half-circle" viewBox="0 0 106 57">
            <path d="M102 4c0 27.1-21.9 49-49 49S4 31.1 4 4"></path>
          </svg>
          <div class="author-name">
            <div class="author-name-prefix">Author</div> {{ cliente.nombre }}
          </div>
        </div>
        <div class="tags">
          <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal"
            @click="abrirModal('editar',cliente)">
            Editar
          </button>
          <button type="button" class="btn btn-primary" @click="borrar(cliente.id)">Borrar</button>
          <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="abrirModal('crear')">
            Crear
          </button>
        </div>
      </div>
    </li>
  </ul>
  <!-- Modal -->
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">{{ modoModal === 'editar' ? 'editar cliente' : 'Crear cliente' }}</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body" >
          <input type="text" v-model="nombre" placeholder="nombre">
          <input type="text" v-model="ape" placeholder="apellido">
          <input type="text" v-model="curp" placeholder="curp">
          <input type="date" v-model="fecha" placeholder="fecha">
        </div>

        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary" @click=" modoModal === 'editar' ? editar() : agregar() " >Save changes</button>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios'


export default {
  name: "ConsumoApi",
  data() {
    return {

      clientes: [],
      nombre: "",
      ape: "",
      curp: "",
      fecha: "",
      modoModal: "",
      clienteId: ""
    }

  },
  mounted() {

    this.obtenerClientes()
  }
  ,
  methods: {
    abrirModal(modal, cliente) {
      
      if(modal === 'editar'){
          this.modoModal = 'editar'
          this.nombre = cliente.nombre;
          this.ape = cliente.apellido;
          this.curp = cliente.curp;
          this.fecha = cliente.fecha;
          this.clienteId = cliente.id;
          

        }else if(modal === 'crear'){
          this.modoModal = 'crear'
          this.nombre = "";
          this.ape = "";
          this.curp = "";
          this.fecha = ""

        }
    },


    async obtenerClientes() {
      axios.get("http://localhost:8081/cliente/Consulta")
        .then(res => {
          console.log(res);
          this.clientes = res.data
        })
        .catch((error) => console.log(error))
    },

    async agregar() {
      const body = {
        nombre: this.nombre,
        apellido: this.ape,
        curp: this.curp,
        fecha: this.fecha

      }
      axios.post("http://localhost:8081/cliente/registrar", body)
        .then(() => this.obtenerClientes())
        .catch((error) => console.log(error))
      this.estcrear = false
    },


    async borrar(id) {
      axios.delete(`http://localhost:8081/cliente/${id}`)
        .then(res => { console.log(res), this.obtenerClientes() })
        .catch((error) => console.log("error en delete", error))

    },
    async editar () {  
     
      
          const body = {
            nombre: this.nombre,
            apellido: this.ape,
            curp: this.curp,
            fecha: this.fecha

          }
          console.log("editando")
          axios.put(`http://localhost:8081/cliente/actualizar/${this.clienteId}`, body )
        .then(() => this.obtenerClientes())
        .catch((error) => console.log(error))

    }
   
  }

}

</script>
<style scoped>
.card {
  display: flex;
  position: relative;
  flex-direction: column;
  height: 350px;
  width: 190px;
  min-width: 250px;
  padding: 1rem;
  border-radius: 16px;
  background: #17141d;
  box-shadow: -1rem 0 3rem #00000067;
  transition: .2s;
  font-family: 'Inter', sans-serif;
  align-content: center;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.card:hover {
  transform: translateY(-0.4rem);
}

.card-author {
  position: relative;
  display: grid;
  grid-template-columns: 75px 1fr;
  align-items: center;
  margin: 3rem 0 0;
}

.author-name {
  color: #7a7a8c;
}

.author-name-prefix {
  color: #e52e71;
  font-weight: 600;
}

.author-avatar span {
  display: block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #f2f2f2;
  margin: 16px 10px;
}

.half-circle {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 60px;
  height: 48px;
  fill: none;
  stroke: #ff8a00;
  stroke-width: 8;
  stroke-linecap: round;
}

.card-header {
  margin-bottom: auto;
  color: #7a7a8c;
}

.card-header p {
  font-size: 14px;
  margin: 0 0 1rem;
  color: #7a7a8c;
}

.card-header .title {
  font-size: 25px;
  margin: .25rem 0 auto;
  cursor: pointer;
  font-family: 'Arial Black', sans-serif;
}

.card-header .title:hover {
  background: linear-gradient(90deg, #ff8a00, #e52e71);
  text-shadow: none;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.tags {
  margin: 1rem 0 2rem;
  padding: .5rem 0 1rem;
  line-height: 2;
  margin-bottom: 0;
}

.tags a {
  font-style: normal;
  font-weight: 700;
  color: #7a7a8c;
  text-transform: uppercase;
  font-size: .66rem;
  border: 3px solid #28242f;
  border-radius: 2rem;
  padding: .2rem .85rem .25rem;
  position: relative;
}

.tags a:hover {
  background: linear-gradient(90deg, #ff8a00, #e52e71);
  text-shadow: none;
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
  -webkit-box-decoration-break: clone;
  box-decoration-break: clone;
  background-clip: text;
  border-color: white;
}
</style>