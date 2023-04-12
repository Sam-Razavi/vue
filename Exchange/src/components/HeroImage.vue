<template>
    <div class="hero-image">
      <img :src="imageSrc" alt="exchange money" />
    </div>
  </template>

<script>
export default {
  data() {
    return {
        imageSrc: ('/src/assets/img/hero.jpeg'),
    };
  },
};
</script>

  <style scoped>
  .hero-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
}
  </style>
