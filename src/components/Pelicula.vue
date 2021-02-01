<template>
  <article class="article-item" id="article-template">
        <div class="image-wrap">
          <img
            :src="pelicula.image"
            :alt="pelicula.title"
            :title="pelicula.title"
          />
        </div>
        <h2>{{ pelicula.title }}</h2>
        <span
          class="date"
          v-bind:class="{
            yeargreen: pelicula.year >= 2019,
            yearred: pelicula.year < 2019,
          }"
        >
          {{ pelicula.year }}
        </span>
        <a href="#">Leer Más</a>
        <input type="button" value="Marcar Favorita" v-on:click="pasarFavoritaAlPadre(pelicula)"/>
        <!--Limpiar Flotado-->
        <div class="clearfix"></div>
      </article>
</template>

<script>
export default {
  name: "Peliculas",
  props:['pelicula'],
  data() {
    return {}
  },
  methods:{
    pasarFavoritaAlPadre(pelicula){
      console.log(pelicula);
      this.$emit('favorita', pelicula);
    }
  }
}
</script>