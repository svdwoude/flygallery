<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      permanent
      clipped
      :mini-variant="miniVariant"
      width="270"
      app
    >
      <v-list>
        <v-list-tile avatar>
          <v-list-tile-avatar>
            <img id="logo" src="~/assets/img/logo_full.png"/>
          </v-list-tile-avatar>
        </v-list-tile>
        <v-list-item class="sidebar-item"
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <!-- <v-icon color="#8b0e3a">{{ item.icon }}</v-icon> -->
            <img class="menu-logo" src="~/assets/img/logo_red.png"></img>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      clipped-left
      app
      color="#01395e"
    >
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>

      <v-spacer />
      <v-toolbar-title v-text="title" />
      <v-spacer />

    </v-app-bar>
    <div ref="scrollContainer" id="container" @mousedown="mousedownHandler">
      <v-main>
        <v-container >
          <Nuxt />
        </v-container>
      </v-main>
    </div>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  methods: {
    mousedownHandler(e) {
      const ele = document.getElementById('container');
      const pos = {
        // The current scroll
        top: window.scrollY,
        // Get the current mouse position
        y: e.clientY,
      };
      ele.style.scrollBehavior = 'auto';
      ele.style.cursor = 'grab';
      e.cancelBubble = true;
      if (e.stopPropagation) e.stopPropagation();


      const mouseUpHandler = function() {
        ele.removeEventListener('mousemove', mouseMoveHandler);
        ele.removeEventListener('mouseup', mouseUpHandler);

        ele.style.cursor = 'default';
        ele.style.removeProperty('user-select');
      };
      const mouseMoveHandler = function(e) {
        console.log('mouse move', e)
        if (document.selection) {
          console.log("remove selection")
          document.selection.empty()
        } else {
          console.log("remove  all ranges")
          window.getSelection().removeAllRanges()
        }


        // How far the mouse has been moved
        const dy = e.clientY - pos.y;

        // Scroll the element
        window.scrollTo(0, pos.top - dy);
        console.log(`scrollTo ${window.scrollY}`)
      };
      ele.addEventListener('mousemove', mouseMoveHandler);
      ele.addEventListener('mouseup', mouseUpHandler);

    },

  },
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [

        {
          icon: 'mdi-crop-square',
          title: 'Klassieke Vliegenramen',
          to: '/'
        },
        {
          icon: 'mdi-door-open',
          title: 'Vliegendeuren',
          to: '/vldr'
        },
        {
          icon: 'mdi-gesture-swipe-right',
          title: 'Schuifvliegenramen',
          to: '/sfvl'
        },
        {
          icon: 'mdi-window-closed-variant',
          title: 'Raamplissé',
          to: '/rmpl'
        },
        {
          icon: 'mdi-door',
          title: 'Deurplissé',
          to: '/drpl'
        },
        {
          icon: 'mdi-script-outline',
          title: 'Rolplissé',
          to: '/rlpl'
        },


      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Blader door ons fotoalbum'
    }
  }
}
</script>

<style>
#logo {
  max-width: 265px;
}

.sidebar-item {
  padding-top: 20px;
  padding-bottom: 20px;
}

.menu-logo {
  max-width: 25px;
  max-height: 25px;
}
</style>
