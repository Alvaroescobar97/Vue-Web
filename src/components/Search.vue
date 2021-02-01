<template>
  <div>
    <Slider :texto="'Busqueda: '+searchString" />

    <div class="center">
      <section id="content">
        <h1 class="subheader" v-if="articles">Articulos encontrados</h1>
        <h1 class="subheader" v-else>Sin Resultados</h1>

        <div id="articles" v-if="articles">
          <Articles v-bind:articles="articles" />
        </div>

        <div v-else>
          No hay articulos que coincidan con tu busqueda
        </div>

      </section>
      <Sidebar />
      <div class="clearfix"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Slider from "./Slider.vue";
import Sidebar from "./Sidebar.vue";
import Articles from "./Articles.vue";
import {Global} from '../Global';

export default {
  name: "Search",
  components: {
    Slider,
    Sidebar,
    Articles
  },
  mounted() {
    this.searchString = this.$route.params.searchString;
    this.getArticlesBySearch(this.searchString);
  },
  data() {
    return {
      url: Global.url,
      articles: null,
      searchString: null
    };
  },
  methods: {
    getArticlesBySearch(searchString) {
      axios.get(Global.url+"search/"+searchString).then((res) => {
        if (res.data.status == "success") {
          this.articles = res.data.articles;
          console.log(this.articles);
        }
      });
    },
  },
};
</script>