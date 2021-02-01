
<template>
  <section id="content">
    <h2 class="subheader">Mi componente</h2>
    <h2>{{ subtitulo }}</h2>
    <hr />

    <h2>Reactividad</h2>
    <h2>{{ nombre }}</h2>
    <input type="text" v-model="nombre" />

    <pre>{{ $data }}</pre>


    <h2>Directivas condicionales</h2>
    <input type="number" v-model="edad" />
    <h2>Mi edad es: {{ nombre }}</h2>
    <p style="color:red;font-weigth: bold;" v-if="edad && edad >= 18 && edad<65">Eres mayor de edad</p>
    <p style="color:green;font-weigth: bold;" v-else-if="edad && edad >65">Eres un adulto mayor</p>
    <p style="color:purple;font-weigth: bold;" v-else>Eres menor de edad</p>

    <h2>Directivas iterativas</h2>
    <ol>
      <li v-for="pelicula in peliculas" v-bind:key='pelicula'>{{pelicula}}</li>
    </ol>

  </section>
</template>

<script>
export default {
  name: "MiComponente",
  data() {
    return {
      subtitulo: "Subtitulo",
      nombre: "",
      edad:18,
      peliculas:['Batman vs superman','Soul','Spiderman','Ozark','Godzilla vs Kong']
    };
  },
};
</script>