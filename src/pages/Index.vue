<template>
  <q-page class="q-pa-xs fit column content-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-full.svg"
    >
    <h1>About this app</h1>
    <p>
      This is an SPA made with <a href="https://miragejs.com">Mirage</a> and
      <a href="https://quasar.dev">Vue</a>. It has two routes to help demonstrate
      how Mirage's in-memory database enables realistic data fetching and
      persisting during a single application session.
    </p>
    <p>
      Mirage's state is reset whenever the application is reloaded.
    </p>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex'
}
</script>
