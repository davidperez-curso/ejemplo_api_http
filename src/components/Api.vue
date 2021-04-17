<template>
  <div>
      <ul v-for="item of peliculas" :key="item.id" >
          <li>La pelicula "{{item.nombre}}" fue dirigida por {{item.director}}</li>
      </ul>
        <hr>
      <div>
        <input type="text" placeholder="Nombre de película" v-model="movie">
        <input type="text" placeholder="Nombre de director" v-model="director"> 
        <select  v-model="tipo">
            <option v-for="tipo of clasificaciones" :key="tipo.id">{{tipo.nombre}}</option>
        </select>

        <button @click.prevent="enviarPost()">Enviar Info</button>
      </div>

        <hr>

        <input type="number" v-model="indice" placeholder="id de película para borrar">
        <button @click.prevent="eliminarDato()">borrar</button>
      
        <hr>
      <pre>
          {{peliculas}}
      </pre>
  </div>
</template>

<script>
export default {
    data(){
        return{
            //aqui se guardan los datos obtenidos con GET
            peliculas: "",
            clasificaciones: "",

            //datos para enviar con el método POST
            movie: "",
            director: "",
            tipo: "",

            indice: ""  
        }
    },

    created(){
        //El método de solicitud GET se usa para obtener los datos de la API 
        
        this.$http.get("peliculas").then(res =>{    // http://localhost:3000/peliculas
            this.peliculas = res.body
        });

        this.$http.get("clasificaciones").then(res =>{  //http://localhost:3000/clasificaciones
            this.clasificaciones = res.body
        });
    },

    methods: {
        //El método de solicitud POST se usa para enviar los datos a la API de backend
        enviarPost(){
            const id = this.peliculas.id ++
            const postDatos = {id: id, nombre: this.movie, director: this.director, clasificacion: this.tipo};

            this.$http.post("peliculas", postDatos).then(res => {  // http://localhost:3000/peliculas
                console.log(res.body);
            })
        },

        //El método delete se usa para eliminar datos de la API
        eliminarDato(){
            const id = this.indice  

            this.$http.delete("peliculas" + "/" + id).then(res => {
                console.log(res.body);
            })
        }
    }

}
</script>

<style>

</style>