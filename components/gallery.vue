<template>
  <div>
    <v-carousel hide-delimiters cycle interval="2000">
      <v-carousel-item v-for="(image,i) in images" :key="i" :src="image.pathLong">
      </v-carousel-item>
    </v-carousel>
    <v-container fluid grid-list-md>
      <!-- xs = 600px full screen (12) -->
      <!-- md = 600px or more. half of the screen (6) -->
      <v-layout row wrap>
        <v-flex xs12 md3 v-for="image in images">
          <v-card>
            <v-img :src="image.pathLong" height="200px" contain></v-img>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import VueGridLayout from 'vue-grid-layout';

export default {
  name: 'gallery',
  components: {
    GridLayout: VueGridLayout.GridLayout,
    GridItem: VueGridLayout.GridItem
  },
  props: ['category'],
  data() {
    return {
      images: [],
    };
  },

  mounted() {
    const files = require.context(`../assets/img/`, true, /\.jpeg$/)
    this.importAll(files);
  },

  methods: {
    importAll(r) {
      r.keys().filter(s => s.includes(`${this.category}/`)).forEach(key => (this.images.push({ pathLong: r(key), pathShort: key })));
    },
  },
}

</script>
