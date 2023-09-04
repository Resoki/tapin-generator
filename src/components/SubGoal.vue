<template>
  <div>
    <div class="sub-goal-header">
      <img class="img-reso" src="../../public/images/reso.jpg"/>
      <p>{{ count }}/500 followers</p>
    </div>
    <div class="w3-border">
      <div class="w3-grey" style="height: 24px;" :style="{ width: progressStyle }"></div>
    </div>
    <button class="subscribe-button" @click="redirectToTwitter">S'abonner à Reso</button>
  </div>
</template>

<script>
export default {
  name: 'SubGoal',
  props: {
    count: Number,
  },
  computed: {
    progressStyle() {
      const percentage = (this.count / 500) * 100;
      return `${percentage}%`;
    },
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
  border-top: 1px solid grey;
  margin-top: 10px;
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
}

.subscribe-button {
  background-color: rgb(101, 72, 231);
  color: white;
  border: none;
  padding: 10px 20px;
  margin-top: 10px;
  cursor: pointer;
}

.subscribe-button:hover {
  background-color: rgb(98, 60, 211);
}
</style>
