<template>
    <div>
    <div v-if="isOpenAvisBox" class="send-avis">
      <h1 class="form-title">Envoyer un avis</h1>
      <form @submit.prevent="sendAvis">
        <div class="form-group">
          <input type="text" id="avisName" placeholder="Nom" v-model="avisName" />
        </div>
        <div class="form-group">
          <div class="rating">
            <span
              v-for="star in 5"
              :key="star"
              :class="['star', star <= avisNote ? 'filled' : '']"
              @click="setRating(star)"
            >★</span>
          </div>
        </div>
        <div class="form-group avis-text-group">
          <textarea id="avisText" placeholder="Avis" v-model="avisText"></textarea>
          <div v-if="avisText.length < 200" class="char-counter">
          {{ avisText.length }}/200
        </div>
        <div v-if="avisText.length > 200" class="char-counter-red">
          {{ avisText.length }}/200
        </div>
        </div>
        <button type="submit">Envoyer</button>
      </form>
    </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'SendAvis',
    props: {
        isOpenAvisBox: Boolean
    },
    data() {
      return {
        avisName: '',
        avisText: '',
        avisNote: 0,
        isOpenAvis: false
      };
    },
    methods: {
      openAvis(){
        this.isOpenAvis = !this.isOpenAvis;
      },
      setRating(rating) {
        this.avisNote = rating;
      },
      sendAvis() {
        console.log(localStorage.getItem('alreadyRated'))
        // if(localStorage.getItem('alreadyRated')) {
        //     console.log('set item')
        //     return alert('Tu as déjà donné ton avis !');
        // }
        const formData = {
          name: this.avisName,
          note: this.avisNote,
          message: this.avisText,
          date: new Date(),
        };
        if(formData.message.length > 200) {
            return alert('Ton message doit etre de 200 caractère max')
        }
        if(formData.name.length < 1) {
            return alert('Tu dois renseigner un nom')
        }
        if(!formData.note) {
            return alert('Tu dois renseigner un note')
        }
        if(!formData.message) {
            return alert('Tu dois renseigner un message')
        }
        // Effectuez une requête POST avec Axios
        axios
          .post('https://reso-site-962417506479.herokuapp.com/record-avis', formData)
          .then((response) => {
            console.log('Avis envoyé avec succès', response);
            // Réinitialisez les champs du formulaire après l'envoi
            this.avisName = '';
            this.avisText = '';
            this.avisNote = 0;
            localStorage.setItem('alreadyRated', true);
            this.$emit('avis-envoye', formData);
          });
      },
    },
  };
  </script>
  
  <style scoped>
.send-avis {
    margin-top: 10px !important;
    margin-bottom: 10px !important;
    margin: 0 auto;
    border: 1px solid black;
    padding: 20px;
    max-width: 500px;
    border-radius: 90px;
    background-color: rgb(63 44 146);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-right: 10px;
    margin-left: 10px;
}

.button-open {
    margin-top: 10px;
}

.form-title {
    font-size: 1.5rem;
    color: white;
    margin: 0;
}

.form-group {
    margin-bottom: 20px;
}

.char-counter-red {
    color: red;
}

.star {
    cursor: pointer;
    font-size: 2.6rem;
    color: gray;
}

.filled {
    color: gold;
}

.avis-text-group {
    margin-bottom: 20px;
}

#avisText {
    width: 100%;
    height: 100px; /* Adjust the height as needed */
    resize: vertical;
    background-color: #f3f3f3;
    border-radius: 12px;
}

#avisName {
background-color: #f3f3f3;
border-radius: 12px;
}

button {
background-color: #007BFF;
color: white;
border: none;
padding: 10px 20px;
cursor: pointer;
border-radius: 5px;
font-size: 1rem;
}
</style>
  