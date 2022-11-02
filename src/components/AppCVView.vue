<template>
  <div class="card card-w70">
    <h3 v-if="!blocks.length">Добавьте первый блок, чтобы увидеть результат</h3>
    <component
      v-else
      v-for="block in blocks"
      :is="'app-c-v-' + block.type"
      :key="block.id"
      v-bind="{ value: block.value }"
    ></component>
  </div>
</template>

<script>
import AppCVTitle from './AppCVComponents/AppCVTitle.vue'
import AppCVSubtitle from './AppCVComponents/AppCVSubtitle.vue'
import AppCVText from './AppCVComponents/AppCVText.vue'
import AppCVAvatar from './AppCVComponents/AppCVAvatar.vue'

export default {
  props: {
    blocks: Array
  },
  components: {
    AppCVTitle,
    AppCVSubtitle,
    AppCVText,
    AppCVAvatar
  }
}
</script>

<style></style>
