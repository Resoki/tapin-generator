<template>
  <button @click="copyTextToClipboard" class="copy-button">Copier</button>
</template>

<script>
export default {
  name: 'CopyComponent',
  props: { textCopy: String },
  methods: {
    copyTextToClipboard() {
      const textArea = document.createElement("textarea");
      textArea.value = this.textCopy;
      document.body.appendChild(textArea);
      textArea.select();
      document.execCommand("copy");
      document.body.removeChild(textArea);
      alert("Texte copié dans le presse-papiers : " + this.textCopy);
    }
  }
};
</script>

<style scoped>
.copy-button {
  margin-top: 10px !important;
  display: flex;
  font-size: 14px;
  align-items: center;
  margin: 0 auto;
  background-color: #ff9800;
  border: none;
  border-radius: 12px;
  padding: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  text-decoration: none;
  color: white;
}

.copy-button img {
  width: 24px;
  height: 24px;
}

.copy-button:hover {
  background-color: #e68a00;
}
</style>
