<template>
  <div>
    <div class="center">
      <section id="content">
        <article class="article-item article-detail" v-if="article">
          <div class="image-wrap">
            <img
              :src="url + 'get-image/' + article.image"
              :alt="article.title"
              v-if="article.image"
            />
            <img
              :src="'https://saveabandonedbabies.org/wp-content/uploads/2015/08/default.png'"
              :alt="article.title"
              v-if="!article.image"
            />
          </div>
          <h1 class="subheader">{{ article.title }}</h1>
          <span class="date">{{ article.date | moment("from", "now") }}</span>
          <p>{{ article.content }}</p>
          <!--Limpiar Flotado-->
          <div class="clearfix"></div>

          <router-link :to="'/editar/' + article._id" class="btn btn-warning"
            >Editar</router-link
          >
          <a @click="deleteArticle(article._id)" class="btn btn-danger"
            >Eliminar</a
          >
        </article>
      </section>
      <Sidebar />
      <div class="clearfix"></div>
    </div>
  </div>
</template>

<script>
import Sidebar from "./Sidebar.vue";
import { Global } from "../Global";
import axios from "axios";
import Swal from "sweetalert2";

export default {
  name: "Article",
  components: {
    Sidebar,
  },
  data() {
    return {
      url: Global.url,
      article: null,
    };
  },
  mounted() {
    var articleId = this.$route.params.id;
    this.getArticle(articleId);
  },
  methods: {
    getArticle(articleId) {
      axios.get(this.url + "article/" + articleId).then((res) => {
        if (res.data.status == "success") {
          this.article = res.data.article;
        }
      });
    },
    deleteArticle(articleId) {
      Swal.fire({
        title: "Estas seguro?",
        text: "Al eliminar el elemento no podrás recuperarlo",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Si, eliminar!",
      }).then((result) => {
        if (result.isConfirmed) {
          axios.delete(this.url + "article/" + articleId).then((res) => {
            if (res.data.status == "success") {
              Swal.fire({
                icon: "success",
                title: "Articulo Eliminado",
                text: "Articulo eliminado correctamente",
              });
              this.$router.push("/blog");
            }
          });
          Swal.fire("Eliminado!", "archivo eliminado", "success");
        } else {
          Swal.fire("Ufff!", "Casi", "warning");
        }
      });
    },
  },
};
</script>