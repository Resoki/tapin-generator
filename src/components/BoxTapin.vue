<template>
  <div v-if="contentDisplay" class="box-tapin">
    <button @click="onClosePopup" class="close-button">X</button>
    <p>{{ contentDisplay }}</p>
    <img alt="Image Tap-in" class="img-display" :src="imgDisplay" />
    <button class="download-button">Télécharger l'image</button>RAUBERTFINANCE
    <div class="button-container">
      <a :href="twitterShareUrl" target="_blank" class="share-button">
        Tweeter
        <img alt="Share Button" src="../../public/images/share.png" />
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BoxTapin',
  props: { contentDisplay: String , imgDisplay: String, twitterShareUrl: String },
  methods: {
    onClosePopup() {
      this.$emit('close-popup');
    }
  }
}
</script>

<style scoped>
.box-tapin {
  position: relative;
  color: black;
  margin-top: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  align-items: center; 
  justify-content: center;
  margin: 0 auto;
  margin-top: 10px !important;
  max-width: 400px;
  background-color: #f5f5f5;
  margin-bottom: 10px;
}

.button-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 10px;
}

.close-button {
  position: absolute;
  top: 2px;
  right: 10px;
  background: none;
  border: none;
  font-size: 20px;
  font-weight: 600;
  cursor: pointer;
  color: rgb(216, 83, 83);
}

.close-button:hover {
  color: red;
}

.img-display {
  border: 2px solid black;
  border-radius: 12px;
}

.box-tapin p {
  margin: 5px 0;
  font-size: 16px;
  line-height: 1.5;
}

.box-tapin img {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
}

.share-button {
  margin-top: 10px !important;
  display: flex;
  font-size: 14px;
  align-items: center;
  margin: 0 auto;
  background-color: #1da1f2;
  border: none;
  border-radius: 12px;
  padding: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  text-decoration: none;
  color: white;
}

.share-button img {
  width: 24px;
  height: 24px;
}

.share-button:hover {
  background-color: #1991da;
}

p {
  font-weight: 600;
  font-size: 14px;
}

.download-button {
  background-color: #4caf50;
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  text-decoration: none;
  color: white;
  font-weight: bold;
  margin: 0 auto;
  margin-top: 10px;
}

.download-button:hover {
  background-color: #45a049;
}

.copy-button {
  margin-left: 10px;
  margin-right: 4px;
  display: flex;
  font-size: 14px;
  align-items: center;
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
