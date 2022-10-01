<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      permanent
      clipped
      :mini-variant="miniVariant"
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon color="#8b0e3a">{{ item.icon }}</v-icon>
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


      <v-toolbar-title v-text="title" />
      <v-spacer />

    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-door',
          title: 'Deurplissé',
          to: '/'
        },
        {
          icon: 'mdi-crop-square',
          title: 'Klassieke Vliegenramen',
          to: '/klvl'
        },
        {
          icon: 'mdi-window-closed-variant',
          title: 'Raamplissé',
          to: '/rmpl'
        },
        {
          icon: 'mdi-script-outline',
          title: 'Rolplissé',
          to: '/rlpl'
        },
        {
          icon: 'mdi-gesture-swipe-right',
          title: 'Schuifvliegenramen',
          to: '/sfvl'
        },
        {
          icon: 'mdi-door-open',
          title: 'Vliegendeuren',
          to: '/vldr'
        },
        {
          icon: 'mdi-movie',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Gentse Vliegenramen'
    }
  }
}
</script>
