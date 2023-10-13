<template>
  <div>
    <div v-if="!isGenerating" class="separator"></div>
    <div class="box-footer-name">
      <p>Tu veux voir ton tapin ici ?<br> Envoit un DM à <b>
        <a class="link-reso" href="https://twitter.com/resoquibug" target='_blank'>@resoquibug</a></b>
      </p>
      <div class="separator-medium"></div>
      <p>@2023 - Créé par <a class="link-reso" href="https://twitter.com/resoquibug" target='_blank'>Reso🛸</a></p>
      <img alt="Logo Twitter @resoquibug" class="img-reso" src="../../public/images/reso.jpg"/>
    </div>
  </div>
</template>

<script>

export default {
  name: 'FooterComponent',
  props: { isGenerating: Boolean }
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
  margin-top: 10px;
}

.separator-medium {
  width: 25%;
}
</style>
