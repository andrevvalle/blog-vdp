<template>
  <v-app>

    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>BLOG</span>
        <span class="font-weight-light">EXPERIMENT</span>
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn
        flat
        href="#"
        target="_blank"
      >
        <span class="mr-2">Github Link</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <ListPosts/>
    </v-content>

  </v-app>
</template>

<script>
import ListPosts from './components/posts'

export default {
  name: 'App',
  components: {
    ListPosts
  }
}
</script>
