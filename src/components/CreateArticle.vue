<template src="./CreateArticle.html"></template>

<script>
import Sidebar from "./Sidebar.vue";
import { Global } from "../Global";
import axios from "axios";
import { required } from "vuelidate/lib/validators";
import Article from "../models/Article";
import Swal from 'sweetalert2';

export default {
  name: "CreateArticle",
  components: {
    Sidebar,
  },
  data() {
    return {
      article: new Article("", "", null, ""),
      url: Global.url,
      submitted: false,
      file: "",
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
  mounted() {},
  methods: {
    save() {
      this.submitted = true;
      this.$v.$touch();
      if (this.$v.$invalid) {
        return false;
      } else {
        axios
          .post(this.url + "save", this.article)
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
                      icon: 'success',
                      title: 'Articulo Creado',
                      text: 'Articulo creado correctamente'
                    });


                      this.article = response.data.article;
                      this.$router.push("/blog");
                    } else {
                      //mostrar alerta de error
                      Swal.fire({
                      icon: 'error',
                      title: 'Creación fallida',
                      text: 'El articulo no se ha guardado bien'
                    });
                    }
                  })
                  .catch((err) => {
                    console.log(err);
                  });
              }else{
                Swal.fire({
                      icon: 'success',
                      title: 'Articulo Creado',
                      text: 'Articulo creado correctamente'
                    });

                this.article = res.data.article;
                      this.$router.push("/blog");
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