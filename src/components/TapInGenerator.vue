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
          <span class="count tapin-click"> {{ clickCountTapin + clickCountPessi }}<span :v-if="clickIncrement.length" class="total-clicks">{{ clickIncrement }}</span> </span>
          <span class="total-clicks"></span>
        </p>
      </div>
      <div class="statistic">
        <p>Nb de visites: <span class="count">{{ visiteCount }}</span></p>
      </div>
  </div>
    <p v-if="!contentDisplay && !isGenerating">Clique sur un des boutons pour générer un tap-in ou un texte pessi</p>
    <ButtonGenerate class="button-hover-animation" :isGenerating="isGenerating" image="/images/graphique.png" text="Générer Tap-In" @emitClick="searchUser()"/>
    <ButtonGenerate class="button-hover-animation color-test" :isGenerating="isGenerating" image="/images/pleurs.png" text="Générer texte Pessi" @emitClick="searchPessiText()"/>
    <GeneratingText :isGenerating="isGenerating" text="Generating..."/>
    <div class="separator-medium"></div>
    <BoxTapin @close-popup="closePopup()" :contentDisplay="contentDisplay" :imgDisplay="imgDisplay" :twitterShareUrl="twitterShareUrl" />
    <PessiText @close-popup="closePopup()" :contentDisplay="contentDisplayPessi"  :twitterShareUrl="twitterShareUrlPessiText"/>
    <SubGoal :isLoadedApi="isLoadedApi" :count="countApi" userTwitter="resoquibug" userName="Reso🛸"/>

    <div>
      <button class="button-open" @click="openAvisBox()">✉️ Envoyer un avis</button>
      <button class="button-close" v-if="isOpenAvisList" @click="openAvisList()">🔒 Fermer les avis</button>
      <button class="button-open" v-if="!isOpenAvisList" @click="openAvisList()">👀 Voir les avis</button>
    </div>
    <SendAvis :isOpenAvisBox="isOpenAvisBox" />
    <AvisBox :isOpenAvisList="isOpenAvisList" :allAvisTab="allAvisTab"/>
    <FooterComponent :isGenerating="isGenerating"/>
  </div>
</template>

<script>
// Components
import FooterComponent from '@/components/FooterComponent.vue';
import ButtonGenerate from '@/components/ButtonGenerate.vue';
import GeneratingText from '@/components/GeneratingText.vue'
import BoxTapin from '@/components/BoxTapin.vue';
import PessiText from '@/components/PessiText.vue';
import SubGoal from '@/components/SubGoal.vue';
import SendAvis from '@/components/SendAvis.vue';
import AvisBox from '@/components/AvisBox.vue';

// Modules
import axios from 'axios';

// Array with all tap-ins
import arrayTapin from '../arrayTapin.json';

export default {
  name: 'TapInGenerator',
  components: { FooterComponent, ButtonGenerate, GeneratingText, BoxTapin, PessiText, SubGoal, SendAvis, AvisBox },
  props: { title: String },
  beforeUnmount() {
    this.displayedIndices = [];
  },
  created() {
    this.refreshFollowCount();
    const hasVisited = localStorage.getItem('visitedWebsite');
    if (!hasVisited) {
      axios.get(this.recordVisiteLink);
      localStorage.setItem('visitedWebsite', true);
    }
    this.tapinStock = this.tapin.length;
    this.pessiStock = this.textPessi.length;
    axios.get(this.clickLinkTapin).then((res)=> this.clickCountTapin = res.data.clickCount);
    axios.get(this.clickLinkPessi).then((res)=>this.clickCountPessi = res.data.clickCount);
    axios.get(this.visiteLink).then((res)=> this.visiteCount = res.data.visiteCount);
  },
  data() {
    return {
      allAvisTab: [],
      isOpenAvisBox: false,
      isOpenAvisList: false,
      isOpenAvis: false,
      clickIncrement: '',
      tapin: arrayTapin,
      textPessi: ['Un sentiment de malaise,de gène 😵‍💫 travers mon corps 🙊 je ne sais pas quoi dire.Peut-être que je devrais te conseiller de supprimer ce tweet😅ou bien te démontrer a quel point tu es un Flop’Zer 🏴‍☠️⚔️ Mais bon, la vie est fait ainsi soit du coule 🤙🏼soit tu deviens un finito 🥵', 
      'ahah 🥶🥶🥶🔥🔥🔥ton tweet il était tellement long que j’ai eu le temps de finir one piece😹😹😹🔥🔥🔥🤟🤟même mon daron il a abuse pas autant des emojis 😹😹🤡🤡🔥🔥🔥🥶🥶mais les termes sont utilisés 😁😁😁🔥🔥🔥🔥🥶🥶', 
      'dignité ? noblesse ? honneur ? fierté ? bravoure ? amour propre ? sagesse ? estime de sois ? pudeur ? chasteté ? élégance ? perception de soi ? grandeur?maturité ? virilité ? réputation ? valeur ? gloire ? prestance ? prestige ? respectabilité ? tenue ? délicatesse ?audace ?',
      'gênant',
      'Masterclass',
      'LES PRODUITS LAITIERS SONT NOS AMIS POUR LA VIE 🐄💀',
      'Qui ne saute pas nest pas pessi !',
      'Aaahhh gaaaarss 🤣😭😭🥷🥷💸twitter c pas du skate 🛹🛹🛹🛹🛹🛹🛹la vraie question c est ce que tiktok ils ont autant la dalle que comment nous on a la dalle 👿💯💥🔥👻👺🥵😖😖 twitter c cartel de Calí 🇲🇽🇲🇽ça tire pas à blanc 🔫👿🔥💥',
      'Hé Akhy😼😼 is le Goat🤩🤩 Tentends 👂🔉🔉ce flop endiablé😈😈 Ça ça fait danser🕺🕺 les Pessi😹🤙🤙 C parti✅ Pas dpanique❌ sur Twitter🐦 Les Akhy😼et les Pessi🤩 dabord✅ Pas dpanique😱❌ sur Twitter🐦 C les Goat😹🤙🤙 C les Goat😹🤙Qui sont là pour ça😼💪',
      'Fai gaffe jsui turc 🇹🇷🇹🇷🇹🇷🇹🇷🇹🇷🇹🇷👌👌👌👌👌👌👌🐺🐺🐺🐺🐺🐺👍👍👍👍👍',
      'MASTERCLASS fuuumier🤣📷📷📷📷📷📷identitada confirmada : ratpi📷📷📷ON PEUT DIRE TAS STONKS?? 📷📷📷belle b*te cogno📷📷📷📷📷-bril📷',
      'NAAAN le raclo il dis les termes 😱😱🤯🥶😹🥕 le DUCZER du 92iiiiiii noserai pas qualifier des termes ainsi 👀🤙🏼🤙🏼🤪💩☎️ Al-Kaïda 😹😹🔫🔫💣🧨 NOOON chakal c NWAAAR 🧑🏿‍🦲🧑🏿‍🦲🧑🏿‍🦲 yiiih racismo no 🇧🇷cogno 😹😹🤙🏼🥶 Armaaand le téléphone sonne 😹😹📞 oui allô ? a lhuile 😹 c NWAAR🌋',
      'Maître TD, sur un arbre perché, Tenait en son bec un tap-in. Daminflop, par lodeur alléché, Lui tint à peu près ce langage : Bonjour, Monsieur TD man. Que vous êtes à votre prime ! que vous me semblez matrixé !', 
      'Séquestré par Pristiano penaldo je sors du silence:  Il a une école de penalty à penaldo city il fais travailler de force les jeunes qui le nomment penaldo. Il nous maltraite , nous dit des gros mots, il dit qu’il est le goat, je suis traumatisée moi et mes compagnons.',
      'MASTERCLASS fuuumier🤣🤣🤣⚔🏴‍☠️les termes employes sont vreuuuumant qualifiés de concis⚔🤣⚔🤣🏴‍☠️identitada confirmada : ratpi🏴‍☠️🏴‍☠️🤣🤙🤙ON PEUT DIRE TAS STONKS?? 📈📈🤣⚔belle b*te cogno🤣🏴‍☠️on ma dit t où?🤷‍♂️🤣🤣dans un magasin de basket?🤣🤣🏴‍☠️🏴‍☠️jleur ai dit non🤣🤣⚔-bril🏴‍☠️😹',
      'Hello sista💁‍♀️, jte demande pas ton numéro📷 On tas déjà dit que tétais une beauté numérique📷 Jfais du real Hip Hop comme les mecs en Amérique📷  Je suis dans la boite, japerçois mon gars Wisla📷 💨',
      'frauduleux moi ? sérieusement ^^ haha on me lavait pas sortie celle la depuis loooongtemps  demande a mes potes si je suis frauduleux tu vas voir les réponses que tu vas te prendre XD rien que la semaine passée jétais à mon prime donc chuuuuut ferme la fraude de merde',
      'Le coffre est plein cousin 🔫📷📷 Viens, on y va maintenant, viens, on y va maintenant, viens, on y va maintenant📷📷📷Regarde-moi bien, cousin📷📷 🔥⚠️🆘🥵🔫',
      'Et jsuis posé dans le club 🥵normal 📷 Et jlève mon verre 📷 Ouais jsuis posé dans lclub📷normal 📷à tous ceux qui sortent pas 📷 Et tu lèves un bras📷, deux bras📷 (eh, eh)',
      'Chiale Chouine Couine Pleure Huhule Miaule Rugit Aboie Hurle Crie Zinzinule Sanglote Pleurniche Chigne Murmure Larmoie Braie Geint Beugle Jacasse Souffle Siffle Ricane Boude Caquette Chuchote Ronronne Roucoule Zozote Gueule Égosille Bêle Chahute Criaille Feule Zougoule',
      'Le coffre est plein cousin 🔫⛓⚠️📛 Viens, on y va maintenant, viens, on y va maintenant, viens, on y va maintenant😡❌🥶‼️Regarde-moi bien, cousin🔥⚠️Je vais pas tregarder, moi, jvais tbombarder, c direct la bagarre🆘🥵🔫',
      'Un jour je serais 🤖 le meilleur Pessi 👨‍🦲👨‍🦲 Je me battrais 🥋 sans répit 😴😴😴 Je ferais tout pour être vainqueur 🏆🏆🏆🏆 et gagner les défis ⛹️⛹️⛹️ je parcourrais 🏃 la Terre entière 🌍🌍🌍🌍 combattant avec espoir 🙏🏾🙏🏾 les Pessi et leurs mystères 🤫🤫🤫🤫',
      'Fb et reste digne akhy',
      'Qui sont les pessi? Qui sommes nous? Des génies venus dailleurs, des monstres goatesque, des extraterrestres ultra-talentueux, des intellectuels, une communauté soudée et efficace ne cédant pas sous loppression. Nous sommes dignes. Chouine en silence. #PessiLivesMatter',
      'Et soudain, le malaise se fit sentir. Drôle de sentiment que d’être subitement gêné par le commentaire d’une fraude comme celle ci-dessus. Ce fut une expérience glacante..', 
      'il y a 2 266 100 Pessis et 37 ministres au gouvernement. Donc si les Pessis décident denvahir lÉlysée, chaque ministre doit combattre 60 174 Pessis',
      'tu es immensément frauduleux + supprime + ratio + gênant + miaule sans jacasser',
      'Feur Feuse Fé Fure Chi Drado Resma Driceps Drilatère Drado D Drupède Tuor Druplé De neuf Ffage Artz La La Lumpur Terback Dragénaire Drilataire Druple Dricolore Ker Gliarella Tuor Ttro Dalajara Ffé Ncer Dri Drillion Drillage Drisyllabe Rteron Drireacteur',
      'En géographie, #elle est mon monde. En histoire, #elle est mon roi. En chimie, #elle est ma solution. En maths, #elle est mon équation. En astronomie, #elle est mon univers Pour les aveugles, #elle est la lumière. Pour les malheureux, #elle est le bonheur.'],
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
      recordClickPessiLink: 'https://reso-site-962417506479.herokuapp.com/record-click-pessi',
      refreshSubLink: 'https://api.socialcounts.org/twitter-live-follower-count/resoquibug',
      avisLink: 'https://reso-site-962417506479.herokuapp.com/get-avis',
      countApi: 0,
      isLoadedApi: false
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
    openAvisList() {
      this.isOpenAvisList = !this.isOpenAvisList;
      this.isOpenAvisBox = false;
      axios.get(this.avisLink).then((res)=> this.allAvisTab = res.data.reverse());
    },
    openAvisBox() {
      this.isOpenAvisBox = !this.isOpenAvisBox;
      this.isOpenAvisList = false;
    },
    objetAleatoire(tabEmpty, tab) {
      let iRd = Math.floor(Math.random() * tab.length);
      while (tabEmpty.includes(iRd)) iRd = Math.floor(Math.random() * tab.length);
      tabEmpty.push(iRd);
      return tab[iRd];
    },
    refreshFollowCount() {
      axios.get(this.refreshSubLink).then((res)=> this.countApi = res.data.API_sub);
      this.isLoadedApi = true;
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
        this.clickIncrement = '+1';
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
        setTimeout(() => this.clickIncrement = '', 1000);
      } catch (error) {
        console.error('Error:', error);
      }
  },
    async searchPessiText() {
      try {
        await axios.get(this.recordClickPessiLink);
        const response2 = await axios.get(this.clickLinkPessi);
        this.clickCountPessi = response2.data.clickCount;
        this.clickIncrement = '+1';
        this.isGenerating = true;
        this.contentDisplay = null;
        this.contentDisplayPessi = null;
        this.imgDisplay = null;

        if (this.displayedIndicesPessi.length === this.textPessi.length) this.displayedIndicesPessi= [];

        setTimeout(() => {
          const objetAlea = this.objetAleatoire(this.displayedIndicesPessi, this.textPessi);
          this.txt = objetAlea;
          this.contentDisplayPessi = objetAlea;
          this.isGenerating = false;
        }, 1000);
        setTimeout(() => this.clickIncrement = '', 1000);
      } catch (error) {
        console.error('Error:', error);
      }
    }
  }
}
</script>
<style scoped>
.total-clicks {
  font-weight: bold;
  font-size: 16px;
  color: #1da1f2; 
  margin-left: 5px;
}

.edit {
  font-size: 10px !important;
}

.button-hover-animation {
  transition: transform 0.2s ease-in-out;
}

.button-hover-animation:hover {
  transform: scale(0.95);
}
.title {
  background-image: linear-gradient(to right, violet, blue);
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  border: 1px;
  font-weight: bold;
  font-size: 32px;
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
  font-weight: bold;
}

.twitter-button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(29, 161, 242, 0.4);
}

.button-close {
  background-color: red;
}

.x-logo {
  width: 92px;
}

.color-test {
  background-color: rgb(246, 186, 7) !important;
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

button {
  margin-top: 10px;
  margin: 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 1rem;
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
  max-width: 350px;
  margin: 0 auto;
  text-align: center;
  display: flex;
  flex-direction: row;
  margin-bottom: 10px;
  font-family: Montserrat, sans-serif;
  color: black;
  box-shadow:
  inset blue 0 0 0 5px, 
  inset rgb(128 62 168) 0 0 0 10px;
  text-shadow: 1px 1px 1px black;
}

.statistic {
  margin-bottom: 10px;
}

.statistic p {
  font-size: 12px;
  color: white;
}

.count {
  font-weight: bold;
  font-size: 18px;
  color: #1da1f2;
  margin-left: 5px;
}
</style>
