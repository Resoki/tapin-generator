<template>
    <div>
      <div v-if="isOpenAvisList" class="avis-list">
        <h2>({{ allAvisTab.length }}) avis</h2>
        <div v-for="(avis, index) in allAvisTab.slice(0, maxAvis)" :key="index" class="avis-item">
          <div class="avis-header">
            <h2 class="avis-title">{{ avis.name }}</h2>
            <p class="avis-date">{{ formatAvisDate(avis.date) }}</p>
          </div>
          <p class="avis-message">{{ avis.message }}</p>
          <div class="avis-rating">
            <span v-for="star in 5" :key="star" :class="['star', star <= avis.note ? 'filled' : '']">★</span>
          </div>
        </div>
        <div v-if="lengthAvis > maxAvis">et ({{ lengthAvis - maxAvis }}) autres avis...</div>
      </div>
    </div>
</template>
<script>
  export default {
    name: 'AvisBox',
    props: { allAvisTab: Array, isOpenAvisList: Boolean },
    data() {
    return {
      displayedAvis: [],
      numAvisToDisplay: 3, 
      maxAvis: 5,
      isOpenAvis: false,
      lengthAvis : this.allAvisTab.length
    };
  },
    methods: {
        openAvis() {
            this.isOpenAvis = !this.isOpenAvis;
        },
        closeAvis() {
            this.isOpenAvis = false;
        },
        formatAvisDate(date) {
            const avisDate = new Date(date);
            const currentDate = new Date();
            const timeDifference = currentDate - avisDate;
            const secondsDifference = Math.floor(timeDifference / 1000);
    
            if (secondsDifference < 60) {
            return `il y a ${secondsDifference} s`;
            } else if (secondsDifference < 3600) {
            const minutesDifference = Math.floor(secondsDifference / 60);
            return `il y a ${minutesDifference} min`;
            } else if (secondsDifference < 86400) {
            const hoursDifference = Math.floor(secondsDifference / 3600);
            return `il y a ${hoursDifference} h`;
            } else {
            const daysDifference = Math.floor(secondsDifference / 86400);
            return `il y a ${daysDifference} j`;
            }
        }
    }
  };
</script>
  
<style scoped>
.avis-list {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.avis-item {
  border: 1px solid #ccc;
  padding: 20px;
  margin: 10px;
  width: 70%;
  max-width: 500px;
  background-color: #3d546a;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
  
.avis-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
  
.avis-title {
  font-size: 1.5rem;
  color: white;
}

.button-close {
  background-color: red;
}

.avis-date {
  font-size: 0.8rem;
  color: grey;
}
  
.avis-message {
  font-size: 1rem;
  margin-top: 10px;
  color: white;
  word-wrap: break-word; 
}

button {
  margin-top: 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
  font-size: 1rem;
}

.avis-rating {
  display: flex;
  justify-content: center;
}
  
.star {
  cursor: pointer;
  font-size: 1.5rem;
  color: gray;
  margin-right: 5px;
}
  
.filled {
  color: gold;
}
  </style>
  