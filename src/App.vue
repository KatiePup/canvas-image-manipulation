<template>
  <p>
    <button @click="backImg()">background</button>
  </p>
  <p>
    <canvas ref="canvas" width="300" height="300"></canvas>
  </p>
</template>

<script>
export default {
  data() {
    return {
      imageUrls: {
        background: "https://source.unsplash.com/user/c_v_r/300x300",
      },
      loadedImages: {},
    };
  },
  mounted() {
    for (const [key, url] of this.imageUrls.entries()) {
      const image = new Image(300, 300);
      image.src = url;

      return this.loadImage(key, image);
    }
  },
  methods: {
    loadImage(key, image) {
      const loaded = (this.loadedImages.image[key] = image);
      return loaded;
    },
    getImage(key) {
      return this.loadedImages.image[key];
    },
    backImg: function () {
      // var can = document.getElementById("myCanvas");
      var can = this.$refs.canvas;
      var ctx = can.getContext("2d");
      console.log(this.loadedImages);
      ctx.drawImage(this.getImage("background"), 0, 0);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
