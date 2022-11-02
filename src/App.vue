<template>
  <div class="container column">
    <app-form @add="addBlock"></app-form>
    <app-loader v-if="loadingBlocks"></app-loader>
    <app-c-v-view :blocks="blocks" v-else></app-c-v-view>
  </div>
  <div class="container">
    <p v-if="!comments.length">
      <button class="btn primary" @click="loadComments">
        Загрузить комментарии
      </button>
    </p>
    <app-loader v-if="loadingComments"></app-loader>
    <div class="card" v-if="comments.length && !loadingComments">
      <app-comments-view :comments="comments"></app-comments-view>
    </div>
  </div>
</template>

<script>
import AppForm from './components/AppForm.vue'
import AppCVView from './components/AppCVView.vue'
import AppCommentsView from './components/AppCommentsView.vue'
import AppLoader from './components/AppLoader.vue'

export default {
  data () {
    return {
      blocks: [],
      comments: [],
      loadingBlocks: false,
      loadingComments: false
    }
  },
  mounted () {
    this.loadBlocks()
  },
  methods: {
    async loadBlocks () {
      try {
        this.loadingBlocks = true
        const response = await fetch(
          'https://vue-cv-generator-default-rtdb.firebaseio.com/blocks.json'
        )
        const data = await response.json()
        this.blocks = Object.keys(data).map((key) => {
          return { id: key, ...data[key] }
        })
        this.loadingBlocks = false
      } catch (error) {
        this.loadingBlocks = false
        throw new Error(error)
      }
    },
    async addBlock (block) {
      try {
        const response = await fetch(
          'https://vue-cv-generator-default-rtdb.firebaseio.com/blocks.json',
          {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              ...block
            })
          }
        )
        const data = await response.json()
        this.blocks.push({ ...block, id: data.name })
      } catch (error) {
        throw new Error(error)
      }
    },
    async loadComments () {
      try {
        this.loadingComments = true
        const response = await fetch(
          'https://jsonplaceholder.typicode.com/comments?_limit=42'
        )
        const data = await response.json()
        this.comments = data
        this.loadingComments = false
      } catch (error) {
        this.loadingComments = false
        throw new Error(error)
      }
    }
  },
  components: {
    AppForm,
    AppCVView,
    AppCommentsView,
    AppLoader
  }
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
