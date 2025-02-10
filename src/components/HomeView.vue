<template>
<div id="app" class="container mt-5 mb-5 col-lg-6 col-xl-6 mx-auto col-sm-12 col-xs-12">
    <div class="mt-5 mb-5">
        <h1 class="text-center text-dark">Actividad integradora 1. CRUD en Vue.js</h1>
    </div>
    <!-- Seccion agregar nueva actividad -->
    <div class="form-group">
        <input type="text" v-model="nuevaActividad" class="form-control" id="crud" placeholder="Escribe una nueva actividad" @keyup.enter="agregarActividad">
        <div class="form-group mt-3 mb-3 text-center">
            <button class="btn btn-success" @click="agregarActividad">Agregar <Icon icon="si:add-fill" width="24" height="24" /></button>       
        </div>
    </div>
    <!-- Seccion actividades agregadas -->
    <ul class="list-group">
        <li v-for="(actividad,index) in actividades" :key="index" class="list-group-item mt-1 mb-1">
            {{ actividad.text }}    
            <div class="positionabsolute">
                <button @click="editarActividad(index)" class="btn btn-primary">
                    <Icon icon="line-md:edit-twotone" width="20" height="20" />
                </button>
                <button @click="eliminarActividad(index)" class="btn btn-danger">
                    <Icon icon="streamline:delete-1-solid" width="14" height="14" />
                </button>
            </div>
        </li>
    </ul>
    <!-- Seccion modificacion de actividades -->
    <div class="form-group mt-5" v-if="editandoActividad !== null">
       <input type="text" class="form-control" v-model="actividadEditada" @keyup.enter="actualizaActividad">
        <div class="form-group mt-3 mb-3 text-center">
            <button @click="actualizaActividad" class="btn btn-primary">
                Actualizar
            </button>
            <button @click="cancelarActividad" class="btn btn-danger">
                Cancelar
            </button>
        </div>
    </div>
</div>
</template>
<script>
// Libreria para lo Iconos utilizados
import { Icon } from '@iconify/vue';

export default{
    data(){
        return {
            nuevaActividad: "",
            actividades : JSON.parse(localStorage.getItem("actividades")) || [],
            editandoActividad: null,
            actividadEditada: "",
        }
    },
    methods: {
            // Funcion agregar nueva actividad
            agregarActividad: function(){
                
                if(this.nuevaActividad.trim() === "") {
                    return;
                }
                this.actividades.push({text: this.nuevaActividad});
                this.guardarActividad();
                this.nuevaActividad = "";
            },
            // Funcion editar actividad
            editarActividad: function(index) {
                this.editandoActividad = index;
                this.actividadEditada = this.actividades[index].text;
            },
            // Funcion eliminar actividad
            eliminarActividad: function(index) {
                if(confirm("Confirmas eliminar la actividad ?")) {
                    this.actividades.splice(index,1);
                    this.guardarActividad();
                }
                
            },
            // Funcion actualiza actividad
            actualizaActividad: function() {
                if (this.actividadEditada.trim() === "") return;
                this.actividades[this.editandoActividad].text = this.actividadEditada;
                this.guardarActividad();
                this.editandoActividad = null;
                this.actividadEditada = "";  
            },
            // Funcion cancelar modificacion de actividad
            cancelarActividad: function() {
                this.editandoActividad = null;
            },
            // Funcion guardar actividad
            guardarActividad: function(){
                localStorage.setItem("actividades", JSON.stringify(this.actividades));
            }

    },
    components: {
       Icon,
   },
};

</script>

<style> 
        /*Hoja de estilo adicional al Bootstrap 5 */
*{
    margin: 0;
    padding: 0;
}
.positionabsolute{
    position: absolute;
    float: right;
    right: 20px;
    top: 3px;

}
button{
    outline: none;
    border: none;
    appearance: none;
    background: transparent;
    margin-left: 5px;
    
}
.list-group-item{
    background: #dcdcdc7d;
    padding: 10px;
}
.list-group-item+.list-group-item{
    border-top-width: 1px;
}
.list-group-item:first-child{
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}
.list-group-item:last-child{
    border-bottom-right-radius: 10px;
    border-bottom-left-radius: 10px;
}
</style>