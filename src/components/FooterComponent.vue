<template>
  <div class="hello">
    <div class="box-footer-name">
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
</style>
