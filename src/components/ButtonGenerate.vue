<template>
    <button v-if="!isGenerating" class="twitter-button" @click="emitClick()">{{text}}</button>
</template>

<script>
export default {
  name: 'ButtonGenerate',
  props: {
    isGenerating: Boolean,
    text: String
  },
  methods: {
    emitClick() {
        this.$emit('emitClick')
    }
  }
}
</script>
<style scoped>
p {
  font-size: 13px;
}
</style>
