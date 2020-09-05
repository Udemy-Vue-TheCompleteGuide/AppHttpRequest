<template>
  <div id="app">
    <h2>Posts</h2>
    <table class="table table-striped" v-if="posts.length > 0">
      <thead>
      <tr>
        <th>Title</th>
        <th>Content</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="post in posts" :key="post.id">
        <td>{{ post.title }}</td>
        <td>{{ post.content }}</td>
      </tr>
      </tbody>
    </table>
    <p v-else>No Data</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      posts: []
    }
  },
  components: {},
  methods: {
    getPosts() {
      return axios.get('https://blog-abc.firebaseio.com/posts.json')
          .then((res) => {
            let posts = [];
            const data = res.data;
            for (const key in data) {
              // eslint-disable-next-line no-prototype-builtins
              if (data.hasOwnProperty(key)) {
                const p = data[key];
                posts.push({id: key, title: p.title, content: p.content});
              }
            }
            return posts;
          });
    }
  },
  created() {
    this.getPosts()
        .then((posts) => {
          this.posts = posts;
        })
        .catch((err) => {
          console.log('My error', err);
        });
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
