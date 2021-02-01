<template src="./CreateArticle.html"></template>

<script>
import Sidebar from "./Sidebar.vue";
import { Global } from "../Global";
import axios from "axios";
import { required } from "vuelidate/lib/validators";
import Article from "../models/Article";
import Swal from "sweetalert2";

export default {
  name: "EditArticle",
  components: {
    Sidebar,
  },
  data() {
    return {
      article: new Article("", "", null, ""),
      url: Global.url,
      submitted: false,
      file: "",
      isEdit: true,
    };
  },
  validations: {
    article: {
      title: {
        required,
      },
      content: {
        required,
      },
    },
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
    save() {
      this.submitted = true;

        var articleId = this.$route.params.id;

      this.$v.$touch();
      if (this.$v.$invalid) {
        return false;
      } else {
        axios
          .put(this.url + "article/" + articleId, this.article)
          .then((res) => {
            console.log(res.data);
            if (res.data.status == "success") {
              //Subida del archivo
              if (
                this.file != null &&
                this.file != undefined &&
                this.file != ""
              ) {
                const formData = new FormData();

                formData.append("file0", this.file, this.file.name);

                var articleId = res.data.article._id;

                axios
                  .post(this.url + "upload-image/" + articleId, formData)
                  .then((response) => {
                    if (response.data.article) {
                      Swal.fire({
                        icon: "success",
                        title: "Articulo Editado",
                        text: "Articulo editado correctamente",
                      });

                      this.article = response.data.article;
                      this.$router.push("/articulo/"+this.article._id);
                    } else {
                      //mostrar alerta de error
                      Swal.fire({
                        icon: "error",
                        title: "Edición fallida",
                        text: "El articulo no se ha editado bien",
                      });
                    }
                  })
                  .catch((err) => {
                    console.log(err);
                  });
              } else {
                Swal.fire({
                  icon: "success",
                  title: "Articulo Editado",
                  text: "Articulo editado correctamente",
                });

                this.article = res.data.article;
                this.$router.push("/articulo/"+this.article._id);
              }
            }
          })
          .catch((err) => {
            console.log(err);
          });
      }
    },
    fileChange() {
      this.file = this.$refs.file.files[0];
      console.log(this.file);
    },
  },
};
</script>