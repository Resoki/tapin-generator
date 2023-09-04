<template>
  <button v-if="!isGenerating" class="twitter-button" @click="emitClick()">
    <img class="img-button twitter-image" :src="image" alt="Image Twitter"/>
    {{ text }}
  </button>
</template>
<script>
export default {
  name: 'ButtonGenerate',
  props: {
    isGenerating: Boolean,
    text: String,
    image: String
  },
  methods: {
    emitClick() {
      this.$emit('emitClick')
    }
  }
}
</script>
<style scoped>
.img-button {
  width: 20px;
}
p {
  font-size: 13px;
}
</style>
