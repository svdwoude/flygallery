<template>
  <div>
    <v-container >
      <masonry-wall :items="images" :column-width="200" :gap="16">
        <template #default="{ item, index }">
          <img class="gallery-item" :src="item.pathLong" @click="selectImage(index)"></img>
        </template>
      </masonry-wall>
    </v-container>
    <v-dialog v-model="dialog" overlay-opacity="0.9" >
        <v-btn class="dialog-close" float-right icon @click="dialog = false">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <v-carousel v-model="selectedImageIndex" height="85vh" hide-delimiters>
          <v-carousel-item contain v-for="(image,i) in images" :key="i" :src="image.pathLong">
          </v-carousel-item>
        </v-carousel>
    </v-dialog>

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
      selectedImageIndex: 0,
      dialog: false
    };
  },

  mounted() {
    const files = require.context(`../assets/img/`, true, /\.jpeg$/)
    this.importAll(files);
  },
  computed: {
    selectedImage() {
      return this.images[this.selectedImageIndex]
    }
  },
  methods: {
    selectImage(index) {
      this.selectedImageIndex = index
      this.dialog = true;
    },
    closeDialog(e) {

    },
    importAll(r) {
      r.keys().filter(s => s.includes(`${this.category}/`)).forEach(key => (this.images.push({ pathLong: r(key), pathShort: key })));
    },
  },
}

</script>

<style>
img.gallery-item {
  max-height: 500px;
  max-width: 200px;
  height: auto;
  width: auto;
}

.full {
  max-height: 80vh;
}

.v-dialog {
  box-shadow: none;
}

.dialog-close {
  float: right;
}
</style>
