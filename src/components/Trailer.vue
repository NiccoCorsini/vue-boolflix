<template>
  <div class="all">
    <iframe
      width="720"
      height="480"
      :src="`https://www.youtube.com/embed/${keyword}`"
      title="YouTube video player"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen
    >
    </iframe>
  </div>
</template>

<script>
export default {
  name: "Trailer",
  props: ["keyword"],
  data() {
    return {};
  },
};
</script>

<style scoped lang="scss">
.all {
  width: 120vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  left: 0;
  background-image: radial-gradient(rgba(0, 0, 0, 0.541), rgb(0, 0, 0));
  iframe {
    transform: translateX(-200px);
    box-shadow: 0 5px 15px 5px rgba(0, 0, 0, 0.479);
  }
}
</style>
