<template>
  <div id="content" >
    <h1>{{ msg }}</h1>
    <h1>{{ texto }}</h1>

    <div style="background: red">
      <MiComponente />

    </div>
  </div>
</template>

<script>
import MiComponente from './MiComponente.vue'

export default {
  name: 'HelloWorld',
  components: {
    MiComponente
  },
  props: {
    msg: String
  },
  data(){
    return {texto: "Hola Mundo Componente!"};
  }
}
</script>

