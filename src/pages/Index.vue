<template>
  <q-page class="">
  <q-spinner
    v-if="loading"
    color="primary"
    size="3em"
  />
      <q-list bordered>
      <q-item
        clickable
        v-ripple
        v-for="post in posts"
        :key="post.data.id"
        @click="showImageDialog(post.data.url, post.data.title)"
      >
        <q-item-section avatar>
          <q-avatar size="70px">
            <img :src="post.data.url">
          </q-avatar>
        </q-item-section>
        <q-item-section>{{ post.data.title }}</q-item-section>
      </q-item>
    </q-list>
    <q-dialog full-width v-model="alert">
      <q-card>
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6">{{ postTitle }}</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section>
        {{ postUrl }}
        </q-card-section>
        <q-card-section>
        <q-img :src="postUrl" />
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script lang="ts">
import Vue from 'vue';

import ExampleComponent from 'components/CompositionComponent.vue';
import { Todo, Meta } from 'components/models';

export default {
  name: 'PageIndex',
  components: { ExampleComponent },
  created() {
    this.$axios.get('https://www.reddit.com/r/awwww.json?raw_json=1')
      .then(response => {
        this.loading = false
        console.log(response)
        this.posts = response.data.data.children
      })
      .catch(error => {
        this.loading = false
        console.log(error);
      })
  },
  data() {
    return {
      posts: [],
      loading: true,
      alert: false,
      address: '',
      postUrl: '',
      postTitle: ''
    }
  },
  methods: {
    showImageDialog(url, title) {
      this.alert = true
      this.postUrl = url
      this.postTitle = title
    }
  }
}

</script>
