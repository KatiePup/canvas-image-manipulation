<template>
  <p>
    <button ref="button" @click="backImg()">background</button>
  </p>
  <p>
    <button ref="button" @click="overImg()">overlay</button>
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
        background: "/images/John300.png",
        shorts: "/images/shorts.png",
      },
      loadedImages: {},
      corners: [[10,10],[196,10],[30,145]],
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
    drawCircle: function(ctx, x, y, radius, fill, stroke, strokeWidth) {
      ctx.beginPath();
      ctx.arc(x, y, radius, 0, 2 * Math.PI, false);
      if (fill) {
        ctx.fillStyle = fill;
        ctx.fill();
      }
      if (stroke) {
        ctx.lineWidth = strokeWidth;
        ctx.strokeStyle = stroke;
        ctx.stroke();
      }
    },
    backImg: function () {
      // var can = document.getElementById("myCanvas");
      var can = this.$refs.canvas;
      var ctx = can.getContext("2d");

      var img = new Image(300, 300);
      img.src = "/images/John-O.svg";

      ctx.drawImage(img, 0, 0);
    },
    arrayFromCoords: function (coords,xi,yi){
      
      let xo = coords[0][0]
      let yo = coords[0][1]

      let a = ( coords[1][0] - xo ) / xi
      let b = ( coords[1][1] - yo ) / xi

      let c = ( coords[2][0] - yo ) / yi
      let d = ( coords[2][1] - yo ) / yi

      return [[xo,yo],[a,b,c,d]]
    },
    overImg: function () {
      var can = this.$refs.canvas;
      var ctx = can.getContext("2d");

      var img = new Image(196, 145);
      img.src = "/images/shorts.png";


      ctx.strokeStyle = "blue";

      //    [ a c ] -> x1 = ax + cy, y1 = by + dx
      //    [ b d ] -> (a, b, c, d)

      var arr = this.arrayFromCoords(this.corners,196,145)
      let [[x, y],[a,b,c,d]] = arr

      ctx.setTransform(a,b,c,d,0,0);
      
      ctx.strokeRect(x,y, 196, 145);
      ctx.drawImage(img, x,y);
      
      ctx.resetTransform();
      this.drawCircle(ctx, 196, 145, 10, 'blue','black',1);
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
