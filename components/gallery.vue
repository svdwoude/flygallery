<template>
  <div id=" ">
    <v-container >
      <masonry-wall :items="images" :column-width="200" :gap="16">
        <template #default="{ item, index }">
          <img class="gallery-item" :src="item.pathLong" @click="selectImage(index)"></img>
        </template>
      </masonry-wall>
    </v-container>
    <v-dialog v-model="dialog" overlay-opacity="0.9" >
        <v-btn class="dialog-close" icon @click="dialog = false">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <div style="clear:both"></div>
        <div class="carousel-container">
          <vueper-slides v-if="dialog" :initSlide="selectedImageIndex + 1" :dragging-distance="50" ref="myVueperSlides" slide-image-inside :bullets="false" fixedHeight="85vh">
            <vueper-slide v-for="(slide, i) in images" :key="i" :title="slide.title" :image="slide.pathLong">
            </vueper-slide>
          </vueper-slides>
        </div>
    </v-dialog>

  </div>
</template>

<script>
import VueGridLayout from 'vue-grid-layout';
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'

export default {
  name: 'gallery',
  components: {
    GridLayout: VueGridLayout.GridLayout,
    GridItem: VueGridLayout.GridItem,
    VueperSlides,
    VueperSlide,
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
      const ele = document.getElementById('container');
      const move = ele.getAttribute("moving")

      if(move !== "yes") {
        console.log("selecting image")
        this.selectedImageIndex = index
        this.dialog = true;
      }
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

.dialog-close {
  /* position: absolute;
  right: 0px;
  top: 60px; */
  float: right;
  cursor: none !important;
}

.v-dialog {
  box-shadow: none;
}

.vueperslides:not(.no-shadow):not(.vueperslides--3d) .vueperslides__parallax-wrapper:after,
.vueperslides:not(.no-shadow):not(.vueperslides--3d) .vueperslides__parallax-wrapper:before {
  box-shadow: none !important;
}

.vueperslides--touchable .vueperslides__track {
  cursor: none !important;
}

.vueperslides__arrow {
  cursor: none !important;
}

.vueperslide__image {
  background-size: contain;
}
</style>
