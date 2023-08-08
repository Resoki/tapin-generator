<template>
  <span v-if="isGenerating">
    <i class="fa fa-spinner fa-spin"/>
    {{text}}
  </span>
</template>

<script>
export default {
  name: 'GeneratingText',
  props: {
    isGenerating: Boolean,
    text: String
  },
}
</script>
<style scoped>
</style>
