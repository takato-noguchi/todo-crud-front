<template>
  <div class="container py-5 position-relative">
    <v-card
      v-for="post in posts" :key="post.id"
      max-width="500"
      class="mx-auto mb-5"
      @click="toShow(post.id)"
    >
      <v-card-text>
        {{ post.content }}
      </v-card-text>
    </v-card>

    <b-button
      v-b-modal.new-modal
      class="position-absolute mt-4 action-btn"
      pill
      variant="primary"
      size="lg"
    >
      +
    </b-button>

    <b-modal
      hide-header
      hide-footer
      id="new-modal"
    >
      <b-form-textarea
        v-model="content"
        autofocus
      />
      <b-form-textarea
        v-model="title"
        autofocus
      />
      <b-button
        class="mt-3"
        variant="primary"
        @click="save()"
      >
        保存
      </b-button>
    </b-modal>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      posts: {
        content: '',
        title: ''
      },
    }
  },
  mounted() {
    this.fetchContents()
  },
  computed: {
    params() {
      return {
        post: {
          content: this.content,
          title: this.title
        }
      }
    }
  },
  methods: { 
    fetchContents() {
      const url = "http://localhost:3000/v1/posts"
      this.$axios.get(url)
        .then((res) => {
          this.posts = res.data
        })
        .catch((err) => {
          console.error(err)
        })
    },
    save() {
      const url = 'http://localhost:3000/v1/posts'
      this.$axios.post(url, this.params)
        .then((res) => {
          this.content = ''
          this.title = ''
          console.log(res)
          this.fetchContents()
        })
        .catch((err) => {
          console.error(err)
        })
    },
    toShow(id) {
      this.$router.push(`/posts/${id}`)
    }
  }
}
</script>