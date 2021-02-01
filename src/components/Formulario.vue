<template>
  <div>
    <Slider texto="Formulario" />

    <div class="center">
      <section id="content">
        <h2 class="subheader">Formulario</h2>

        <form action="" class="mid-form" @submit.prevent="mostrarUsuario()">
          <div class="form-group">
            <label for="nombre">Nombre</label>
            <input type="text" name="nombre" v-model="user.nombre" />
            <div v-if="submitted && !$v.user.nombre.required">Campo requrido</div>
            <div v-if="submitted && !$v.user.nombre.minLength">El Campo requriere mas caracteres</div>
          </div>
          <div class="form-group">
            <label for="apellidos">Apellidos</label>
            <input type="text" name="apellidos" v-model="user.apellidos" />
            <div v-if=" submitted && !$v.user.apellidos.required">Campo requrido</div>
            <div v-if="submitted && !$v.user.apellidos.minLength">El Campo requriere mas caracteres</div>
          </div>
          <div class="form-group">
            <label for="bio">Biografia</label>
            <textarea name="bio" v-model="user.bio"></textarea>
            <div v-if="submitted && !$v.user.bio.required">Campo requrido</div>
            <div v-if="submitted && !$v.user.bio.minLength">El Campo requriere mas caracteres</div>
          </div>
          <div class="form-group radiobuttons">
            <input
              type="radio"
              name="genero"
              value="hombre"
              v-model="user.genero"
            />
            Hombre
            <input
              type="radio"
              name="genero"
              value="mujer"
              v-model="user.genero"
            />
            Mujer
            <input
              type="radio"
              name="genero"
              value="otro"
              v-model="user.genero"
            />
            Otro
          </div>

          <!--Limpiar Flotado-->
          <div class="clearfix"></div>

          <input type="submit" value="Enviar" class="btn btn-success" />
        </form>
        <div class="datos" v-if="user.nombre">
          <h3>{{ user.nombre + " " + user.apellidos + " " + user.genero }}</h3>
        </div>
      </section>
      <Sidebar />
      <div class="clearfix"></div>
    </div>
  </div>
</template>

<script>
import Slider from "./Slider.vue";
import Sidebar from "./Sidebar.vue";
import { required, minLength } from "vuelidate/lib/validators";

export default {
  name: "Formulario",
  components: {
    Slider,
    Sidebar,
  },
  validations: {
    user: {
      nombre: {
        required,
        minLength: minLength(2),
      },
      apellidos: {
        required,
        minLength: minLength(2),
      },
      bio: {
        required,
        minLength: minLength(10),
      }
    },
  },
  methods: {
    mostrarUsuario() {
      console.log(this.user);
      this.submitted = true;

      this.$v.$touch();
      if (this.$v.$invalid) {
        return false;
      }

      alert("Validacion ok");
    },
  },
  data() {
    return {
      submitted: false,
      user: {
        nombre: "",
        apellidos: "",
        bio: "",
        genero: "",
      },
    };
  },
};
</script>