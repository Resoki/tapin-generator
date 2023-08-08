<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <p v-if="!contentDisplay && !isGenerating">Clique sur le button ci dessous pour générer un tap-in !</p>
    <ButtonGenerate 
      :isGenerating="isGenerating" 
      text="Generate Tap-In"
      @emitClick="searchUser()"
    />
    <GeneratingText 
      :isGenerating="isGenerating" 
      text="Generating..."
    />
    <BoxTapin 
      :contentDisplay="contentDisplay" 
      :imgDisplay="imgDisplay" 
      :twitterShareUrl="twitterShareUrl" 
    />
    <FooterComponent />
  </div>
</template>

<script>
// Components
import FooterComponent from '@/components/FooterComponent.vue';
import ButtonGenerate from '@/components/ButtonGenerate.vue';
import GeneratingText from '@/components/GeneratingText.vue'
import BoxTapin from '../components/BoxTapin.vue';

export default {
  name: 'TapInGenerator',
  components: { FooterComponent, ButtonGenerate, GeneratingText, BoxTapin },
  props: {
    title: String
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
        {content: `I need logo to receive my metamask hack wallet !`, url:''},
        {content: `L'eau dans ces gobelets était pas bonne !`, url:'images/gobelet.jpg'},
        {content: `Mes écouteurs quand je les sort de ma poche`, url:'images/ecouteurs.jpg'},
        {content: `On a tous cette fissure`, url:'images/fissure.jpg'},
        {content: `Moi quand je me cogne le coude`, url:'images/coude.jpg'},
        {content: `Une relation c'est 50/50`, url:'images/50.jpg'},
        {content: `Tout le monde aime ce medicament`, url:'images/medic.jpg'}
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

.share-button {
  margin-top: 10px;
  display: flex;
  align-items: center;
  margin: 0 auto;
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
