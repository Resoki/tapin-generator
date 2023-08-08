<template>
  <div class="hello">
    <div class="box-footer-name">
      <p>@2023 - Créé par Reso</p>
      <img class="img-reso" src="../../public/images/reso.jpg"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FooterComponent',
}
</script>
<style scoped>
.img-reso {
  width: 34px;
  height: 34px;
  border-radius: 50%;
  margin-left: 5px;
}

.box-footer-name {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
