<template>
  <div>
    <v-container >
      <masonry-wall :items="images" :ssr-columns="1" :column-width="300" :gap="16">
        <template #default="{ item, index }">
          <img class="gallery-item" :src="item.pathLong" @click="selectImage(item)"></img>
        </template>
      </masonry-wall>
    </v-container>
    <v-dialog v-model="dialog" overlay-opacity="0.9" >
        <v-btn class="dialog-close" icon @click="dialog = false">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <v-img @click="closeDialog" class="full" :src="selectedImage.pathLong" contain> </v-img>

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
      selectedImage: {},
      dialog: false
    };
  },

  mounted() {
    const files = require.context(`../assets/img/`, true, /\.jpeg$/)
    this.importAll(files);
  },

  methods: {
    selectImage(image) {
      this.selectedImage = image;
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
  max-width: 300px;
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
