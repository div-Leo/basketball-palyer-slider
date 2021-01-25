<template>
  <div id="app" :class="player.color">
    <Background />
    <div class="slide__content">
      <Number :current="current" :total="players.length"/>
      <Player :name="player.name" :surname="player.surname" :image="player.image"/>
      <Details :averages="player.Averages" :details="player.Details" :current="current"/>
      <Pagination :current="current" :pages="pagesNumbers" @chage-slide="goToSlide"/>
    </div>
    <CustomCursor :current="current"/>
  </div>
</template>

<script>
  import data from "./assets/data/players.json";
  import Player from "./components/Player";
  import Details from "./components/Details";
  import CustomCursor from "./components/Cursor";
  import Number from "./components/Number";
  import Pagination from "./components/Pagination";
  import Background from "./components/Background";

  export default {
    name: 'App',
    components: {
      Player,
      Details,
      CustomCursor,
      Number,
      Pagination,
      Background
    },
    methods: {
      nextSlide () {
        this.current < this.players.length-1
          ? this.current++
          : this.current = 0;
      },
      newInterval () {
        return setInterval(() => this.nextSlide(), 5000);
      },
      goToSlide (i) {
        clearInterval(this.interval);
        this.interval = this.newInterval();
        if (i < this.players.length) this.current = i;
      },
    },
    computed: {
      player () {
        return this.players[this.current];
      }, 
      pagesNumbers () {
        return this.players.map((_,i) => i);
      }
    },
    data () {
      return {
        players: data,
        current: 0,
        interval: this.newInterval(),
      };
    },
  }
</script>

<style lang="scss">
  @import './assets/scss/colors.scss';
  $base-unit: calc(100vw / 48);

  #app {
    background: $color-bg;
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    height: 100vh;
    position: relative;
  }
  .slide__content {
    display: grid;
    grid-column: 1 / 12;
    grid-template-columns: repeat(12, 1fr);
    width: 100vw;
    position: relative;
    z-index: 1;
  }
  .red {
    --accent: #{$color-red};
    --background: #{$color-red-dark};
  }
  .purple {
    --accent: #{$color-purple};
    --background: #{$color-purple-dark};
  }
  .blue {
    --accent: #{$color-blue};
    --background: #{$color-blue-dark};
  }
  .green {
    --accent: #{$color-green};
    --background: #{$color-green-dark};
  }
</style>
