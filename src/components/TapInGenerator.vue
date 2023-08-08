<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p v-if="!contentDisplay && !isGenerating">Clique sur le button ci dessous pour générer un tap-in !</p>
    <button v-if="!isGenerating" class="twitter-button" @click="searchUser()">Generate Tap-in</button>
    <span v-else><i class="fa fa-spinner fa-spin"></i> Generating...</span>
    <div v-if="contentDisplay" class="box-tapin" >
    <p>{{ contentDisplay }}</p>
    <img :src="imgDisplay" />
      <a :href="twitterShareUrl" target="_blank" class="share-button">
        Tweeter 
        <img src="../../public/images/share.png" alt="Share on Twitter" />
      </a>
    </div>
    <FooterComponent />
  </div>
</template>

<script>
import FooterComponent from '../components/FooterComponent.vue';

export default {
  name: 'TapInGenerator',
  components: {FooterComponent},
  props: {
    msg: String
  },
  data() {
    return {
      tapin: [
        {content: `On a tous déjà vu dans ça dans nos manuels d'Histoire géo`, url:'images/histoiregeo.jpg'},
        {content: `On a tous déjà essayer de mettre ça dans notre cxl`, url:'images/cul.jpg'},
        {content: `J’ai arraché cette puff à un petit qui s’étouffait, heureusement j’étais la`, url:'images/puff.jpg'},
        {content: `Askip je ressemble à Freeze Corleone`, url:'images/askipfreeze.jpg'},
        {content: `Askip je ressemble à maitre gims`, url:'images/gims.jpg'},
        {content: `Askip je ressemble à Mona Lisa`, url:'images/monalisa.jpg'},
        {content: `Je vais faire une détection à Woippy ! J’espère ils me prendront.`, url:'images/woippychoose.jpg'},
        {content: `Vous connaissez ce pays ?? !`, url:'images/listenbourg.jpg'},
        {content: `I need logo to receive my metamask hack wallet !`, url:''}
        ],
      txt: '',
      contentDisplay: '',
      imgDisplay: '',
      isGenerating: false
    }
  },
  computed: {
    twitterShareUrl() {
      const tweetText = encodeURIComponent(this.contentDisplay);
      return `https://twitter.com/intent/tweet?text=${tweetText}`;
    },
  },
  methods: {
    objetAleatoire(tableau) {
      let indexAleatoire = Math.floor(Math.random() * tableau.length);
      while (indexAleatoire === this.lastImageIndex) {
        indexAleatoire = Math.floor(Math.random() * tableau.length);
      }
      
      this.lastImageIndex = indexAleatoire;
      return tableau[indexAleatoire];
    },

    searchUser() {
      this.isGenerating = true;
      this.contentDisplay = null;
      this.imgDisplay = null;

      setTimeout(()=> {
      const objetAlea = this.objetAleatoire(this.tapin);
      this.txt = objetAlea;
      this.contentDisplay = objetAlea.content;
      this.imgDisplay = objetAlea.url;
      this.isGenerating = false;
      }, 1000);
    }
  }
}
</script>
<style scoped>
h3 {
  margin: 40px 0 0;
}

.box-tapin {
  color: black;
  margin-top: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin: 0 auto;
  margin-top: 10px !important;
  max-width: 400px; /* Ajustez la largeur selon vos besoins */
  background-color: #f5f5f5;
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
  margin-top: 10px;
  display: inline-block;
  background-color: #1da1f2;
  border: none;
  border-radius: 50%;
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

.form {
  width: 200px;
  display: flex;
  flex-direction: column;
  margin: 0 auto;
}

.twitter-button {
  background-color: #1da1f2;
  border: none;
  border-radius: 9999px;
  color: white;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.twitter-button:hover {
  background-color: #1991da;
}

.twitter-button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(29, 161, 242, 0.4);
}


.fa-spinner {
  animation: spin 1s infinite linear;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

a {
  color: #42b983;
}


</style>
