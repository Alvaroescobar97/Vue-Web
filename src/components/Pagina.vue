<template>
  <div>
    <div class="center">
      <section id="content">
        <h2 class="subheader">Pagina de Pruebas</h2>
        <h2>{{ id_desde_url }}</h2>
        <h2>{{ nombre }}</h2>
        <button @click="actualizarNombre()">Act Nombre</button>
        <button @click="redirigirAlBlog()">Ir al blog</button>
      </section>

      <Sidebar></Sidebar>
      <div class="clearfix"></div>
    </div>
  </div>
</template>

<script>
import Sidebar from "./Sidebar.vue";
export default {
  name: "Pagina",
  created() {
    console.log("Comp creado !!");
  },
  mounted() {
    console.log("Comp montado !!");
    this.id_desde_url = this.$route.params.id;
  },
  updated() {
    console.log("Comp actualizado !!");
  },
  destroyed() {
    console.log("Comp eliminado !!");
  },
  data() {
    return {
      id_desde_url: null,
      nombre: "Alvaro Escobar",
    };
  },
  methods: {
    actualizarNombre() {
      this.nombre = "F";
    },
    redirigirAlBlog() {
      this.$router.push({ name: "pagina", params: { id: "Redireccion" } });
    },
  },
  components: {
    Sidebar,
  },
};
</script>