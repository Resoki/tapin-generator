<template>
  <div>
    <img class="x-logo" src="../../public/images/x-logo.png" />
    <h1 class="title">{{ title.slice(0,7)}} / <span class="span-pessi">Pessi</span> Generator</h1>
    <div class="statistics-panel">
  <div class="statistic">
    <p>Items en stock: <span class="count">{{ tapinStock + pessiStock }}</span></p>
  </div>
 <div class="statistic">
  <p>Total de clicks: 
    <span class="count tapin-click"> {{ clickCountTapin + clickCountPessi }} </span>

    <span class="total-clicks">
    </span>
  </p>
</div>
  <div class="statistic" v-if="visiteCount > 1">
    <p>Nb de visites: <span class="count">{{ visiteCount }}</span></p>
  </div>
</div>

    <p v-if="!contentDisplay && !isGenerating">Clique sur le button ci dessous pour générer un tap-in ou alors clique sur le deuxième pour générer un texte de pessi !</p>
    <ButtonGenerate :isGenerating="isGenerating" text="Generer un Tap-In" @emitClick="searchUser()"/>
    <ButtonGenerate :isGenerating="isGenerating" text="Generate un texte Pessi" @emitClick="searchPessiText()"/>
    <GeneratingText :isGenerating="isGenerating" text="Generating..."/>
      <div class="separator-medium"> </div>
    <BoxTapin @close-popup="closePopup()" :contentDisplay="contentDisplay" :imgDisplay="imgDisplay" :twitterShareUrl="twitterShareUrl" />
    <PessiText @close-popup="closePopup()" :contentDisplay="contentDisplayPessi"  :twitterShareUrl="twitterShareUrlPessiText"/>
    <FooterComponent :isGenerating="isGenerating"/>
  </div>
</template>

<script>
// Components
import FooterComponent from '@/components/FooterComponent.vue';
import ButtonGenerate from '@/components/ButtonGenerate.vue';
import GeneratingText from '@/components/GeneratingText.vue'
import BoxTapin from '@/components/BoxTapin.vue';
import PessiText from '@/components/PessiText.vue'

// Modules
import axios from 'axios';

//Array with all tap-ins
import arrayTapin from '../arrayTapin.json';

export default {
  name: 'TapInGenerator',
  components: { FooterComponent, ButtonGenerate, GeneratingText, BoxTapin, PessiText},
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
    this.pessiStock = this.textPessi.length;
    axios.get(this.clickLinkTapin).then((res)=> {
      console.log('created clicklink',res.data )
      this.clickCountTapin = res.data.clickCount
    });
    axios.get(this.clickLinkPessi).then((res)=> {
      console.log('created clicklink',res.data )
      this.clickCountPessi = res.data.clickCount
    });
    axios.get(this.visiteLink).then((res)=> this.visiteCount = res.data.visiteCount);
  },
  data() {
    return {
      tapin: arrayTapin,
      textPessi: ['Un sentiment de malaise,de gène 😵‍💫 travers mon corps 🙊 je ne sais pas quoi dire.Peut-être que je devrais te conseiller de supprimer ce tweet😅ou bien te démontrer a quel point tu es un Flop’Zer 🏴‍☠️⚔️ Mais bon, la vie est fait ainsi soit du coule 🤙🏼soit tu deviens un finito 🥵', 
      'ahah 🥶🥶🥶🔥🔥🔥ton tweet il était tellement long que j’ai eu le temps de finir one piece😹😹😹🔥🔥🔥🤟🤟même mon daron il a abuse pas autant des emojis 😹😹🤡🤡🔥🔥🔥🥶🥶mais les termes sont utilisés 😁😁😁🔥🔥🔥🔥🥶🥶', 
      'dignité ? noblesse ? honneur ? fierté ? bravoure ? amour propre ? sagesse ? estime de sois ? pudeur ? chasteté ? élégance ? perception de soi ? grandeur?maturité ? virilité ? réputation ? valeur ? gloire ? prestance ? prestige ? respectabilité ? tenue ? délicatesse ?audace ?',
      'gênant',
      'Masterclass',
      'LES PRODUITS LAITIERS SONT NOS AMIS POUR LA VIE 🐄💀',
      'Aaahhh gaaaarss 🤣😭😭🥷🥷💸twitter c pas du skate 🛹🛹🛹🛹🛹🛹🛹la vraie question c est ce que tiktok ils ont autant la dalle que comment nous on a la dalle 👿💯💥🔥👻👺🥵😖😖 twitter c cartel de Calí 🇲🇽🇲🇽ça tire pas à blanc 🔫👿🔥💥',
      'Hé Akhy😼😼 is le Goat🤩🤩 Tentends 👂🔉🔉ce flop endiablé😈😈 Ça ça fait danser🕺🕺 les Pessi😹🤙🤙 C parti✅ Pas dpanique❌ sur Twitter🐦 Les Akhy😼et les Pessi🤩 dabord✅ Pas dpanique😱❌ sur Twitter🐦 C les Goat😹🤙🤙 C les Goat😹🤙Qui sont là pour ça😼💪',
      'Fai gaffe jsui turc 🇹🇷🇹🇷🇹🇷🇹🇷🇹🇷🇹🇷👌👌👌👌👌👌👌🐺🐺🐺🐺🐺🐺👍👍👍👍👍'],
      txt: '',
      contentDisplay: '',
      contentDisplayPessi: '',
      imgDisplay: '',
      isGenerating: false,
      displayedIndicesTapin: [],
      displayedIndicesPessi: [],
      visiteCount: 0,
      clickCountTapin: 0,
      clickCountPessi: 0,
      tapinStock: 0,
      twitterLink: 'https://twitter.com/intent/tweet?text=',
      visiteLink: 'https://reso-site-962417506479.herokuapp.com/visite',
      clickLinkPessi: 'https://reso-site-962417506479.herokuapp.com/click-pessi',
      clickLinkTapin: 'https://reso-site-962417506479.herokuapp.com/click-tapin',
      recordVisiteLink: 'https://reso-site-962417506479.herokuapp.com/record-visite',
      recordClickTapinLink: 'https://reso-site-962417506479.herokuapp.com/record-click-tapin',
      recordClickPessiLink: 'https://reso-site-962417506479.herokuapp.com/record-click-pessi'
    }
  },
  computed: {
    twitterShareUrl() {
      return `${this.twitterLink}${encodeURIComponent(this.contentDisplay)}`;
    },
    twitterShareUrlPessiText() {
      return `${this.twitterLink}${encodeURIComponent(this.contentDisplayPessi)}`;
    },
  },
  methods: {
    objetAleatoire(tabEmpty, tab) {
      let indexRd = Math.floor(Math.random() * tab.length);
      while (tabEmpty.includes(indexRd)) {
        indexRd = Math.floor(Math.random() * tab.length);
      }
      tabEmpty.push(indexRd);
      return tab[indexRd];
    },
    closePopup() {
      this.contentDisplay = null;
      this.contentDisplayPessi = null;
    },
    async searchUser() {
      try {
       await axios.get(this.recordClickTapinLink);
        const response2 = await axios.get(this.clickLinkTapin);
        this.clickCountTapin = response2.data.clickCount;

        this.isGenerating = true;
        this.contentDisplay = null;
        this.contentDisplayPessi = null;
        this.imgDisplay = null;

        if (this.displayedIndicesTapin.length === this.tapin.length) this.displayedIndicesTapin = [];

        setTimeout(() => {
          const objetAlea = this.objetAleatoire(this.displayedIndicesTapin, this.tapin);
          this.txt = objetAlea;
          this.contentDisplay = objetAlea.content;
          this.imgDisplay = objetAlea.url;
          this.isGenerating = false;
        }, 1000);
      } catch (error) {
        console.error('Error:', error);
      }
  },
      async searchPessiText() {
        try {
          await axios.get(this.recordClickPessiLink);
          const response2 = await axios.get(this.clickLinkPessi);
          this.clickCountPessi = response2.data.clickCount;

          this.isGenerating = true;
          this.contentDisplayPessi = null;
          this.imgDisplay = null;

          if (this.displayedIndicesPessi.length === this.textPessi.length) this.displayedIndicesPessi= [];

          setTimeout(() => {
            const objetAlea = this.objetAleatoire(this.displayedIndicesPessi, this.textPessi);
            this.txt = objetAlea;
            this.contentDisplayPessi = objetAlea;
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
.total-clicks {
  font-weight: bold;
  font-size: 16px;
  color: #1da1f2; /* Couleur Twitter */
  margin-left: 5px;
}

.edit {
  font-size: 10px !important;
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
  margin: 10px;
}

.twitter-button:hover {
  background-color: #1991da;
}

.twitter-button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(29, 161, 242, 0.4);
}

.x-logo {
  width: 92px;
}

.span-pessi {
  background-image: linear-gradient(to right, rgb(156, 109, 63), rgb(246, 186, 7));
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  border: 1px;
}
.separator {
  border: 1px solid grey;
  width: 100%;
  margin-top: 10px;
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
