<template>
  <h2>Комментарии</h2>
  <ul class="list">
    <app-comment
      v-for="comment in comments"
      :key="comment.id"
      :id="comment.id"
      :name="comment.name"
      :email="comment.email"
      :body="comment.body"
    ></app-comment>
  </ul>
</template>

<script>
import AppComment from './AppComment.vue'
export default {
  props: {
    comments: {
      type: Array
    }
  },
  components: {
    AppComment
  }
}
</script>
