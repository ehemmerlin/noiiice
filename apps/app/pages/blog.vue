<template>
  <div id="blog">
    <h1>Blog</h1>
    <div class="blogcontainer">
      <div v-for="(post, key) in posts" :key="key" class="blogpost">
        <router-link :to="'/post/' + post.slug">
          <div v-if="post.featuredImage" class="imagecontainer" :style="{ backgroundImage: 'url(' + post.featuredImage + ')'}" />
          <div v-if="!post.featuredImage" class="imagecontainer nofeature" />
        </router-link>
        <div class="post-text">
          <router-link :to="'/post/' + post.slug">
            <h2 class="posttitle">
              {{ post.title }}
            </h2>
          </router-link>
          <p class="excerpt">
            {{ post.excerpt }}
          </p>
        </div>
      </div>
    </div>
    <!-- <Paginator :per-page="5" :all-content="posts" @setPage="setPage($event)" /> -->
  </div>
</template>

<script>
import { mapMutations } from 'vuex'
// import Paginator from '~/components/Paginator.vue'

export default {
  name: 'Blog',
  head() {
    return {
      title: `Noiice Blog`,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Noiiice Blog. A Serverless blog.'
        }
      ]
    }
  },
  // components: {
  //   Paginator
  // },
  data() {
    return {
      content: []
    }
  },
  async asyncData({ store, getters }) {
    if (!store.state.posts.length) {
      const posts = await store.dispatch('blog/getPosts')
      const filteredPosts = posts.filter((post) => {
        return post.status === 'Published'
      })
      return { posts: filteredPosts }
    }
  },
  methods: {
    ...mapMutations({
      getPosts: 'blog/getPosts'
    }),
    setPage(e) {
      this.content = e.content
      window.scrollTo(0, 0)
    }
  }
}
</script>
