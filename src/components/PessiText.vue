<template>
  <div v-if="contentDisplay" class="box-tapin">
    <button @click="onClosePopup" class="close-button">X</button>
    <p>{{ contentDisplay }}</p>
    <CopyComponent :textCopy="contentDisplay" />
    <a :href="twitterShareUrl" target="_blank" class="share-button">
      Tweeter 
      <img src="../../public/images/share.png" />
    </a>
  </div>
</template>

<script>
// Components
import CopyComponent from '@/components/CopyComponent.vue';

export default {
  name: 'PessiText',
  props: { contentDisplay: String, twitterShareUrl: String },
  methods: {
    onClosePopup() {
      this.$emit('close-popup');
    },
  },
  components: { CopyComponent }
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
</style>
