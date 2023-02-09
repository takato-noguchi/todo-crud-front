<template>
  <div class="container py-5">
    <b-card>
      <b-card-text
        class="show-txt"
      >
        {{ post.content }}
        {{ post.title }}
      </b-card-text>
      <b-button
        size="sm"
        @click="toTop()"
      >
        Topへ
      </b-button>
    </b-card>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      post: {},
    }
  },
  mounted() {
    this.fetchContent()
  },
  methods: {
    fetchContent() {
      const url = `http://localhost:3000/v1/posts/${this.$route.params.id}`
      this.$axios.get(url)
        .then((res) => {
          this.post = res.data.post
        })
        .catch(() => {
          this.toTop()
        })
    },
  },
  toTop() {
    this.$router.push('/posts')
  }
}
</script>

<style scoped>
.show-txt {
  color: #000;
}
</style>