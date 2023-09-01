<template>
  <div>
    <img class="x-logo" src="../../public/images/x.png" />
    <h1 class="title">{{ title }}</h1>
    

    <div class="statistics-panel">
  <div class="statistic">
    <p>Tapins en stock: <span class="count">{{ tapinStock }}</span></p>
  </div>
  <div class="statistic">
    <p>Total de clicks: <span class="count">{{ clickCount }}</span></p>
  </div>
  <div class="statistic" v-if="visiteCount > 1">
    <p>Nb de visites: <span class="count">{{ visiteCount }}</span></p>
  </div>
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

// Modules
import axios from 'axios';

//Array with all tap-ins
import arrayTapin from '../arrayTapin.json';

export default {
  name: 'TapInGenerator',
  components: { FooterComponent, ButtonGenerate, GeneratingText, BoxTapin},
  props: { title: String },
  beforeUnmount() {
    this.displayedIndices = [];
  },
  created() {
    const hasVisited = localStorage.getItem('visitedWebsite');
    if (!hasVisited) {
      axios.get(this.recordVisiteLink);
      localStorage.setItem('visitedWebsite', true);
    }
    this.tapinStock = this.tapin.length;
    axios.get(this.clickLink).then((res)=> this.clickCount = res.data.clickCount);
    axios.get(this.visiteLink).then((res)=> this.visiteCount = res.data.visiteCount);
  },
  data() {
    return {
      tapin: arrayTapin,
      txt: '',
      contentDisplay: '',
      imgDisplay: '',
      isGenerating: false,
      displayedIndices: [],
      visiteCount: 0,
      clickCount: 0,
      tapinStock: 0,
      twitterLink: 'https://twitter.com/intent/tweet?text=',
      visiteLink: 'https://reso-site-962417506479.herokuapp.com/visite',
      clickLink: 'https://reso-site-962417506479.herokuapp.com/click',
      recordVisiteLink: 'https://reso-site-962417506479.herokuapp.com/record-visite',
      recordClickLink: 'https://reso-site-962417506479.herokuapp.com/record-click'
    }
  },
  computed: {
    twitterShareUrl() {
      return `${this.twitterLink}${encodeURIComponent(this.contentDisplay)}`;
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
    async searchUser() {
      try {
        await axios.get(this.recordClickLink);
        const response2 = await axios.get(this.clickLink);
        this.clickCount = response2.data.clickCount;

        this.isGenerating = true;
        this.contentDisplay = null;
        this.imgDisplay = null;

        if (this.displayedIndices.length === this.tapin.length) this.displayedIndices = [];

        setTimeout(() => {
          const objetAlea = this.objetAleatoire(this.tapin);
          this.txt = objetAlea;
          this.contentDisplay = objetAlea.content;
          this.imgDisplay = objetAlea.url;
          this.isGenerating = false;
        }, 1000);
      } catch (error) {
        console.error('Error:', error);
      }
  }
  }
}
</script>
<style scoped>
.count {
  font-size: 16px;
}
.title {
  background-image: linear-gradient(to right, violet, blue);
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  border: 1px;
}
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
  border-radius: 8px;
  text-align: center;
  max-width: 300px;
  font-size: 14px;
  margin: 0 auto;
  margin-top: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  color: white;
  margin-bottom: 10px;
}

.statistics-panel {
  background-color: #3d546a;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 10px;
  max-width: 300px; /* Largeur maximale du panneau */
  margin: 0 auto; /* Pour le centrer horizontalement */
  text-align: center; /* Centrer le texte */
  display: flex;
  flex-direction: row;
  margin-bottom: 10px;
}

.statistic {
  margin-bottom: 10px; /* Espacement entre les statistiques */
}

.statistic p {
  margin: 0;
  font-size: 12px;
  color: white;
}

.count {
  font-weight: bold;
  font-size: 18px;
  color: #1da1f2; /* Couleur Twitter */
  margin-left: 5px;
}

</style>
