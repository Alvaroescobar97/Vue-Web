<template>
  <section>
    <div v-if="!articles">
      <div className="loading">
        <img src="https://media.giphy.com/media/3oEjI6SIIHBdRxXI40/giphy.gif" />
        <p>Esto podría tardar un rato</p>
      </div>
    </div>

    <div id="articles-list" v-if="articles && articles.length >= 1">
      <article
        class="article-item"
        v-for="article in articles"
        :key="article._id"
      >
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

        <h2>
          <router-link :to="{ name: 'article', params: { id: article._id } }">{{
            article.title
          }}</router-link>
        </h2>
        <span class="date">{{ article.date | moment("from", "now") }}</span>
        <router-link :to="{ name: 'article', params: { id: article._id } }"
          >Leer Más</router-link
        >
        <div class="clearfix"></div>
      </article>
    </div>
  </section>
</template>

<script>
//import axios from "axios";
import { Global } from "../Global";

export default {
  name: "Articles",
  props: ["articles"],
  data() {
    return {
      url: Global.url,
    };
  },
};
</script>