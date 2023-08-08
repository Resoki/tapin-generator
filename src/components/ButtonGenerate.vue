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
.img-reso {
  width: 34px;
  height: 34px;
  border-radius: 50%;
  margin-left: 5px;
}

.box-footer-name {
  display: flex;
  justify-content: center;
  align-items: center;
}

.link-reso {
  text-decoration: none;
  color: rgb(4, 119, 234);
}

p {
  font-size: 13px;
}
</style>
