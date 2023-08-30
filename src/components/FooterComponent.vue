<template>
  <div class="hello">
      <div class="separator"> </div>
    <div class="box-footer-name">
      <p>Tu veux voir ton tapin ici ?<br> Envoit un DM à <b>@resoquibug</b></p>
        <div class="separator-medium"> </div>
      <p>@2023 - Créé par <a class="link-reso" href="https://twitter.com/resoquibug" target='_blank'>Reso</a></p>
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

.link-reso {
  text-decoration: none;
  color: rgb(4, 119, 234);
}

p {
  font-size: 13px;
}

.separator {
  border: 1px solid grey;
  width: 100%;
  margin: 10px;
}

.separator-medium {
  width: 25%;
}
</style>
