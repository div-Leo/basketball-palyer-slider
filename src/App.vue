<template>
  <div id="app" :class="player.color">
    <div ref="background" class="slide__background"/>
    <div class="slide__content">
      <div class="slide__number">
        <span class="slide__number-current">
          {{currentSlide}}
        </span>
        <span class="slide__number-total">
          /{{players.length}}
        </span>
      </div>
      <transition 
        @enter="enterText"
        @leave="leaveText"
        >  
        <div ref="nameBorder" :key="player.name" class="slide__player-name slide__player-name--border">
          <h1>{{player.name}}</h1>
          <h1>{{player.surname}}</h1>
        </div>
      </transition>
      <transition 
        @enter="enterText"
        @leave="leaveText"
        > 
        <div ref="nameFIll" :key="player.name" class="slide__player-name slide__player-name--fill">
          <h1>{{player.name}}</h1>
          <h1>{{player.surname}}</h1>
        </div>
      </transition>
      <transition 
        @enter="enterImage"
        @leave="leaveImage" > 
        <div ref="image" :key="playerImage" class="slide__player-image">
          <div>
            <img :src="playerImage" alt="">
          </div>
        </div>
      </transition>
      <Details :averages="player.Averages" :details="player.Details" :current="current"/>
      <div class="slide__pagination">
        <div 
          v-for="(players, i) in players" 
          @click="goToSlide(i)"
          :key="i" 
          class="slide__pagination-line"
          :class="{'slide__pagination-line--current': i === current}">
        </div>
      </div>
    </div>
    <CustomCursor :current="current"/>
  </div>
</template>

<script>
  import data from "./assets/data/players.json";
  import Details from "./components/Details";
  import CustomCursor from "./components/Cursor";
  import { gsap } from "gsap";

  export default {
    name: 'App',
    components: {
      Details,
      CustomCursor,
    },
    methods: {
      enterText(el, done) {
        gsap.from(el.childNodes, {
          height: 0,
          duration: .4,
          delay: .4,
          ease: 'power3.easeOut',
          onComplete: done,
        });
      },
      leaveText(el, done) {
        gsap.to(el.childNodes, {
          height: 0,
          duration: .3,
          ease: 'power3.easeOut',
          onComplete: done,
        });
      },
      enterImage(el, done) {
        gsap.from(el, {
          zIndex: 2,
          y: -70,
          height: 0,
          duration: .7,
          ease: 'power3.easeOut',
          onComplete: done,
        });
      },
      leaveImage(el, done) {
        gsap.to(el, {
          position: 'absolute',
          zIndex: 0,
          paddingTop: 100,
          duration: .7,
          autoAlpha: 1,
          onComplete: done,
        });
      },
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
      currentSlide () {
        return this.current+1;
      },
      playerImage () {
        return require(`./assets/img/${this.players[this.current].image}`);
      },
      player () {
        return this.players[this.current];
      }
    }, 
    mounted() {
      const { image, nameBorder, nameFIll, background  } = this.$refs

      gsap.from(image, {
        scale: .9,
        y: -100,
        opacity: 0,
        duration: 1,
        ease: 'power3.easeOut',
      })
      gsap.from(background,{
          height: 0,
          duration: 1,
          delay: .3,
          ease: 'power3.easeOut',
        },
      );
      gsap.from([nameBorder.childNodes, ...nameFIll.childNodes], {
        height: 0,
        duration: .8,
        ease: 'power3.easeOut',
      });
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
  .slide__number {
    font-family: "Saol Display";
    margin: $base-unit;
    text-align: center;
  }
  .slide__number-current {
    font-size: 3rem;
  }
  .slide__number-total {
    font-size: 1.8rem;
    vertical-align: -50%;
  }
  .slide__background {
    position: absolute;
    grid-column: 1 / 6;
    height: 100%;
    width: 100%;
    background: var(--background);
    transition: background 1s ease;
  }
  .slide__content {
    display: grid;
    grid-column: 1 / 12;
    grid-template-columns: repeat(12, 1fr);
    width: 100vw;
    position: relative;
    z-index: 1;
  }
  .slide__player-name {
    position: absolute;
    font-size: calc(#{$base-unit} * 1.8);
    font-family: "Saol Display";
    text-transform: uppercase;
    margin-top: 50vh;
    transform: translate(0, -50%);
    text-align: right;
    margin-left: calc(#{$base-unit} * 6);
    h1 {
      overflow: hidden;
      margin: 0;
      line-height: 1em;
    }
  }
  .slide__player-name--fill {
    z-index: 1;
  }
  .slide__player-name--border {
    z-index: 3;
    color: transparent;
    -webkit-text-stroke-width: 2.5px;
    -webkit-text-stroke-color: #fff;
    -webkit-font-smoothing: antialiased;
  }
  .slide__player-image {
    position: relative;
    grid-column: 5 / 9;
    height: calc(#{$base-unit} * 20);
    overflow: hidden;
    padding-right: calc(#{$base-unit} * 1);
    z-index: 2;
    & > div {
      height: calc(#{$base-unit} * 20);
    }
    img {
      width:100%; 
      height:100%;
      object-fit: cover;
      object-position: center;
    }
  }
  .slide__pagination {
    display: flex;
    grid-column: 8 / 12;
    align-items: center;
  }
  .slide__pagination-line {
    width: 100%;
    height: 10px;
    border-bottom: solid 2px $color-cursor-circle;
    margin-left: calc(#{$base-unit} * 1);
    transition: all .5s ease;
    cursor: pointer;
  }
  .slide__pagination-line--current {
    border-bottom: solid 4px var(--accent);
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
