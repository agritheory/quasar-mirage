<template>
<q-layout view="lHh Lpr lFf" class="bg-grey-9 text-white">
    <q-header elevated class="bg-grey-6">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          Quasar / Mirage
        </q-toolbar-title>
        <span>{{ $q.platform.is.name + " / " + $q.platform.is.platform }} </span> |
        <span> Mirage v0.1.38</span> |
        <span> Quasar v{{ $q.version }} </span>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      persistent=false
      content-class="bg-grey-8"
    >
      <q-list dark>
        <q-item-label
          header
          class="text-grey-8"
        >
          Essential Links
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink'

export default {
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: [
        {
          title: 'Todos',
          caption: 'Demonstration',
          icon: 'school',
          link: '#/todos'
        },
        {
          title: 'About',
          caption: 'About this App',
          icon: 'public',
          link: '/'
        }
      ]
    }
  }
}
</script>
