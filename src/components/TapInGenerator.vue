<template>
  <div>
    <img class="x-logo" src="../../public/images/x.png" />
    <h1>{{ title }}</h1>
    <div class="generation-counter">
    <p>Nb de générations: <b>{{ clickCount }}</b></p>
    <p v-if="visiteCount > 1">Nb de visite: {{ visiteCount }}</p>
  </div>
    <p v-if="!contentDisplay && !isGenerating">Clique sur le button ci dessous pour générer un tap-in !</p>
    <ButtonGenerate :isGenerating="isGenerating" text="Generate Tap-In" @emitClick="searchUser()"/>
    <GeneratingText :isGenerating="isGenerating" text="Generating..."/>
    <BoxTapin :contentDisplay="contentDisplay" :imgDisplay="imgDisplay" :twitterShareUrl="twitterShareUrl" />
    <FooterComponent />
  </div>
</template>

<script>
// Components
import FooterComponent from '@/components/FooterComponent.vue';
import ButtonGenerate from '@/components/ButtonGenerate.vue';
import GeneratingText from '@/components/GeneratingText.vue'
import BoxTapin from '@/components/BoxTapin.vue';
import axios from 'axios';

export default {
  name: 'TapInGenerator',
  components: { FooterComponent, ButtonGenerate, GeneratingText, BoxTapin },
  props: {
    title: String
  },
  beforeUnmount() {
  this.displayedIndices = [];
},
  created() {
      axios.get('https://reso-site-962417506479.herokuapp.com/record-visite')
        .then((res)=> {
          console.log(res.data);
        })
      axios.get('https://reso-site-962417506479.herokuapp.com/click')
        .then((res)=> {
          this.clickCount = res.data.clickCount;
        })
      axios.get('https://reso-site-962417506479.herokuapp.com/visite')
        .then((res)=> {
          console.log(res.data);
       this.visiteCount = res.data.visiteCount;
        })
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
        {content: `Tout le monde aime ce medicament`, url:'images/medic.jpg'},
        {content: `Fav et commente et je te dis comment je te vois`, url:'images/cmjtevois.jpg'},
        {content: `- T'as mangé ou tu manges avec nous ? 
                  - J'ai pas mangé et je mange pas avec vous`, url:'images/tamange.jpg'},
        {content: `Les petits d’aujourd’hui ils veulent des iPhones alors que moi je voulais ça :`, url:'images/avant.jpg'},
        {content: `En primaire on a tous connu un mec comme ça : `, url:'images/primaire.jpg'},
        {content: `Des « hommes » n’ont jamais habité ici : `, url:'images/400k.jpg'},
        {content: `Les discussions comme ça >>>>>>`, url:'images/discussion.jpg'},
         {content: `Bah heureusement qu’il l’a raté mdrrrr`, url:'images/rateravion.jpg'},
          {content: `Ça c’était trop le sandwich de la Hess mdrrr`, url:'images/sandwich.jpg'},
        ],
      txt: '',
      contentDisplay: '',
      imgDisplay: '',
      isGenerating: false,
      displayedIndices: [],
      visiteCount: 0,
      clickCount: 0
    }
  },
  computed: {
    twitterShareUrl() {
      return `https://twitter.com/intent/tweet?text=${encodeURIComponent(this.contentDisplay)}`;
    },
  },
  methods: {
    objetAleatoire(tab) {
      let indexRd = Math.floor(Math.random() * tab.length);
      while (this.displayedIndices.includes(indexRd)) {
        indexRd = Math.floor(Math.random() * tab.length);
      }
      this.displayedIndices.push(indexRd);
      return tab[indexRd];
  },
    searchUser() {
    axios.get('https://reso-site-962417506479.herokuapp.com/record-click')
      .then((res)=> {
        console.log('new click', res)
      })

      axios.get('https://reso-site-962417506479.herokuapp.com/click')
      .then((res)=> {
        this.clickCount = res.data.clickCount;
      })
      this.isGenerating = true;
      this.contentDisplay = null;
      this.imgDisplay = null;
      if (this.displayedIndices.length === this.tapin.length) {
        this.displayedIndices = [];
      }
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
.twitter-button {
  background-color: #1da1f2;
  border: none;
  border-radius: 12px;
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

.x-logo {
  width: 46px;
}
.generation-counter {
  background-color: #b2b2b2;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 10px;
  text-align: center;
  max-width: 300px;
  margin: 0 auto;
  margin-top: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  color: black;
}
</style>
