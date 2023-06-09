<template>
  <div class="gradient">
    <div class="header">
      <a @click="$emit('main')" class="main">
        <img style="position:absolute;left:10px;top:10px;" src="https://psv4.vkuseraudio.net/s/v1/d/ZOjISoBZgYhf9zOcQaGBsq38YMZwUAFBu2z2BQ147CgUP2y5ynVO_Bdt19mSUYq1aqmPEsH2f-x5vchoWw0FU4yIPNyOXu9PUv8-UsQdz8dFf0R3OWS3eg/logo.png">
      </a>
      <div class="pop">
        <img class="popp" src="https://sun9-37.userapi.com/impg/CV6JO9-Lz7Ne74SVWBiwpiyCFVU4NbEJjTx9cA/_C7pjA1SvWY.jpg?size=47x51&quality=96&sign=f3a7128fd66eb2deabc7b3ddb1886a39&type=album">
        <div class="popf">Популярные уроки</div>
      </div>
      <div class="all">
        <img class="allp" src="https://sun9-64.userapi.com/impg/4l42qxR4_YYC9znDYQBUbnhpenTQfpTeyMX6aA/rOgEu_e589c.jpg?size=47x51&quality=96&sign=b422c1d50d78e85aee7a10b1dd4965ea&type=album">
        <div class="allf" @click="$emit('main')">Все уроки</div>
      </div>
      <div class="fav">
        <img class="favp" src="https://sun9-80.userapi.com/impg/N7Ive0haac7j7Zp9S6c1FlLMf008IKlc4w8eWA/2oBp_cxHPkQ.jpg?size=47x51&quality=96&sign=00931bda687dab269c362c45421b83b8&type=album">
        <div class="favf">Моделирование процессов</div>
      </div>
    </div>

    <div class="teploo">
      <div class="teplo">
        <h1>Замедленное моделирование теплового движения молекул</h1>

        <div class="sliders">
          <div class="slider">
            <label>Давление(Па): {{ pressure }}</label>
            <input type="range" v-model="pressure" min="0" max="100" />
          </div>

          <div class="slider">
            <label>Температура(K): {{ temperature }}</label>
            <input type="range" v-model="temperature" min="0" max="100" />
          </div>

          <div class="slider">
            <label>Объем(м³): {{ volume }}</label>
            <input type="range" v-model="volume" min="0" max="100" />
          </div>
        </div>

        <div class="simulation">
          <canvas ref="canvas"></canvas>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      pressure: 50,
      temperature: 50,
      volume: 50,
      particles: [],
      canvasContext: null,
      animationFrameId: null,
    };
  },
  mounted() {
    this.canvasContext = this.$refs.canvas.getContext("2d");
    this.updateCanvasSize();
    this.startAnimation();
  },
  watch: {
    pressure() {
      this.initializeParticles();
    },
    temperature() {
      this.initializeParticles();
    },
    volume() {
      this.updateCanvasSize();
      this.initializeParticles();
    },
  },
  methods: {
    initializeParticles() {
      this.particles = [];

      const numParticles = Math.floor((this.pressure / 100) * 1000);
      const particleRadius = 2;

      for (let i = 0; i < numParticles; i++) {
        const x =
          Math.random() * (this.$refs.canvas.width - particleRadius * 2) +
          particleRadius;
        const y =
          Math.random() * (this.$refs.canvas.height - particleRadius * 2) +
          particleRadius;
        const dx = (Math.random() - 0.5) * (this.temperature / 25);
        const dy = (Math.random() - 0.5) * (this.temperature / 25); 
        const color = "rgba(255, 255, 255, 0.5)";

        this.particles.push({ x, y, dx, dy, radius: particleRadius, color });
      }
    },

    startAnimation() {
      this.animate();
    },
    stopAnimation() {
      cancelAnimationFrame(this.animationFrameId);
    },
    animate() {
      this.canvasContext.clearRect(
        0,
        0,
        this.$refs.canvas.width,
        this.$refs.canvas.height
      );

      this.particles.forEach((particle) => {
        // Обновление позиции частицы
        particle.x += particle.dx;
        particle.y += particle.dy;

        // Обработка столкновений с границами холста
        if (
          particle.x + particle.radius > this.$refs.canvas.width ||
          particle.x - particle.radius < 0
        ) {
          particle.dx = -particle.dx;
        }
        if (
          particle.y + particle.radius > this.$refs.canvas.height ||
          particle.y - particle.radius < 0
        ) {
          particle.dy = -particle.dy;
        }

        // Рисование частицы
        this.canvasContext.beginPath();
        this.canvasContext.arc(
          particle.x,
          particle.y,
          particle.radius,
          0,
          Math.PI * 2,
          false
        );
        this.canvasContext.fillStyle = particle.color;
        this.canvasContext.fill();
        this.canvasContext.closePath();
      });

      this.animationFrameId = requestAnimationFrame(this.animate);
    },
    updateCanvasSize() {
      const canvas = this.$refs.canvas;
      canvas.width = (this.volume / 100) * 400;
      canvas.height = (this.volume / 100) * 400;
    },
  },
};
</script>
  
  <style src="./styles.css" scoped></style>