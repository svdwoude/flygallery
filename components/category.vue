<template>
  <div>
    <h1>{{title}}</h1>
    <gallery :category="technicalName"></gallery>
  </div>
</template>

<script>
import Gallery from './gallery.vue';

export default {
  name: 'category',
  props: ['title'],
  data() {
    return {
    };
  },
  computed: {
    technicalName() {
      const name = this.title.normalize("NFD").replace(/\p{Diacritic}/gu, "").toLowerCase().replace(/ /g, "_");
      console.log(`Converting ${this.title} to ${name}`)
      return name
    }
  },
}

</script>
