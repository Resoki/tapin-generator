<template>
  <div class="main">
    <div class="sub-goal-header">
      <img class="img-reso" src="../../public/images/reso.jpg"/>
      <p v-if="count === 0" class="follow">Chargement...</p>
      <p v-if="count > 0" class="follow">{{ count }}/{{objective}} <span class="follow2">Followers</span></p>
    </div>
    <div v-if="count > 0" class="w3-border">
      <div class="w3-grey color" style="height: 24px;" :style="{ width: progressStyle }"></div>
    </div>
    <button class="subscribe-button" @click="redirectToTwitter">Suivre Reso🛸</button>
  </div>
</template>

<script>
export default {
  name: 'SubGoal',
  props: {
    count: Number,
    isLoadedApi: Boolean
  },
  data() {
    return {
      objective: 1000
    }
  },
  computed: {
    progressStyle() {
      return `${(this.count / this.objective) * 100}%`;
    },
  },
  created() {
    if(this.count > 1000) this.objective = 1500;
  },
  methods: {
    redirectToTwitter() {
      window.location.href = 'https://twitter.com/resoquibug';
    },
  },
};
</script>

<style scoped>
.sub-goal-header {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 10px;
}

.follow2 {
  font: 14px "TwitterChirp";
  color: #8B98A5;
}


.color {
  border-radius: 10px;
  background-color: #6548e7 !important;
}

.img-reso {
  width: 34px;
  height: 34px;
  border-radius: 50%;
  margin-right: 10px;
}

.w3-border {
  width: 140px;
  margin: 0 auto;
  border-radius: 10px;
}

.main {
  border: 2px solid black;
  width: 200px;
  margin: 0 auto;
  border-radius: 12px;
}

.subscribe-button {
  width: 160px;
  background-color: rgb(101, 72, 231);
  color: white;
  border: none;
  border-radius: 12px;
  padding: 10px 20px;
  margin-top: 10px;
  margin-bottom: 10px;
  cursor: pointer;
}
.subscribe-button:hover {
  background-color: rgb(98, 60, 211);
}
</style>
