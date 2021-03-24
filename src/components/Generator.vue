<template>
  <div>
    ここgenerator
    <canvas
      width="512"
      height="512"
      class="canvas"
      ref="canvas"></canvas>
    <p>name<input type="text" v-model="text"></p>
    <p>radius<input type="range" min="0" max="300" v-model.number="radius"></p>
    <p>y<input type="range" min="300" max="500" v-model.number="y"></p>
    <p>size<input type="range" min="40" max="100" v-model.number="fontsize"></p>
    <p>stroke<input type="range" min="3" max="10" v-model.number="fontstroke"></p>
    <p><input type="file" ref="image" @change="setImage"></p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      text: "なまこずし",
      radius: 200,
      y: 512/12 * 9,
      fontsize: 75,
      fontstroke: 5,
      image: null,
    }
  },
  watch: {
    radius() {
      this.draw()
    },
    y() {
      this.draw()
    },
    fontsize() {
      this.draw()
    },
    fontstroke() {
      this.draw()
    }
  },
  methods: {
    draw() {
      this.ctx.clearRect(0, 0, 512, 512)

      if (this.image != null){
        var clp = document.createElement('canvas')
        clp.width = 512
        clp.height = 512
        var clpctx = clp.getContext('2d')
        clpctx.beginPath()
        clpctx.arc(512/2, 512/2, this.radius, 0, Math.PI * 2)
        clpctx.clip()
        clpctx.drawImage(this.image,0,0, 512, 512)
        this.ctx.drawImage(clp, 0, 0)
      }

      this.ctx.strokeStyle = "pink"
      this.ctx.lineWidth = 14
      this.ctx.beginPath()
      this.ctx.arc(512/2, 512/2, this.radius, 0, Math.PI * 2)
      this.ctx.stroke()

      this.ctx.fillStyle = "pink"
      this.ctx.strokeStyle = "white"
      this.ctx.font = `${this.fontsize}px "M PLUS 1p", sans-serif`
      this.ctx.textAlign = "center"
      this.ctx.textBaseline = "middle"
      this.ctx.fillText(
        this.text,
        512/2, this.y,
        )
      this.ctx.lineWidth = this.fontstroke
      this.ctx.strokeText(
        this.text,
        512/2, this.y,
        )
    },
    setImage(){
      this.readImage(this.$refs.image.files[0])
      this.draw()
    },
    readImage(file) {
      let reader = new FileReader();
      reader.onload = this.loadImage;
      reader.readAsDataURL(file);
    },
    loadImage(e) {
      let image = new Image();
      image.src = e.target.result;
      this.image = image;
    }
  },
  mounted() {
    // mounted 以降で canvas の DOM にアクセスできる
    // CreateJS などを使うときにも、ここで canvas と紐付ける
    // console.log(this.$el)
    this.ctx = this.$refs.canvas.getContext('2d')
    this.draw()
  }
}
</script>
