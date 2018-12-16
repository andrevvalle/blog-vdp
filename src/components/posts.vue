<template>
  <v-container>
    <v-layout
      text-xs-center
      wrap
    >

      <v-timeline
        v-infinite-scroll="loadMore"
        infinite-scroll-disabled="busy"
        infinite-scroll-distance="10"
        infinite-scroll-throttle-delay="400"
      >
        <v-timeline-item
          v-for="(post, key) in data"
          :key="key"
          color="red lighten-2"
          large
        >
          <v-card class="elevation-2">
            <v-card-title class="headline">{{post.title}}</v-card-title>
            <v-card-text>
              {{post.content}}
            </v-card-text>
          </v-card>
        </v-timeline-item>
      </v-timeline>

      <div
        class="timeline-load"
        v-if="busy"
      >
        <v-progress-circular
          indeterminate
          color="primary"
        ></v-progress-circular>
        <span>Carregando novos posts...</span>
      </div>
      
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      loadPerScroll: 5,
      loadPerScrollAccumulator: 5,
      busy: false,
      data: [],
      posts: []
    }
  },
  methods: {
    getPosts() {
      axios.get('http://127.0.0.1:3000/postfeed')
        .then(res => {
          this.posts = res.data;
          this.loadInitialData();
        });
    },
    loadMore() {
      if (this.loadPerScrollAccumulator >= this.posts.length) {
        return false;
      }

      this.busy = true;

      setTimeout(() => {
        if (this.loadPerScrollAccumulator <= this.posts.length) {
          this.loadPerScrollAccumulator += this.loadPerScroll;
        }

        this.data = this.posts.slice(0, this.loadPerScrollAccumulator);
        this.busy = false;
      }, 1000);
    },
    loadInitialData() {
      this.data = this.posts.slice(0, this.loadPerScrollAccumulator);
    }
  },
  mounted() {
    this.getPosts();
  }
}
</script>

<style>
.timeline-load {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 50px;
}

.timeline-load span {
  margin-left: 15px;
}
</style>
