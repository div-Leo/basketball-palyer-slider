<template>
  <div class="slide__player-container">
    <transition 
      @enter="enterText"
      @leave="leaveText"
      >  
      <div ref="nameBorder" :key="name" class="slide__player-name slide__player-name--border">
        <h1>{{name}}</h1>
        <h1>{{surname}}</h1>
      </div>
    </transition>
    <transition 
      @enter="enterText"
      @leave="leaveText"
      > 
      <div ref="nameFIll" :key="name" class="slide__player-name slide__player-name--fill">
        <h1>{{name}}</h1>
        <h1>{{surname}}</h1>
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
  </div>
</template>

<script>
  import { gsap } from "gsap";

  export default {
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
      animateOnLoad () {
        const { image, nameBorder, nameFIll  } = this.$refs
        gsap.from(image, {
          scale: .9,
          y: -100,
          opacity: 0,
          duration: 1,
          ease: 'power3.easeOut',
        })
        gsap.from([nameBorder.childNodes, ...nameFIll.childNodes], {
          height: 0,
          duration: .8,
          ease: 'power3.easeOut',
        });
      },
    }, 
    mounted() {
      this.animateOnLoad()
    },
    computed: {
      playerImage () {
        return require(`../assets/img/${this.image}`);
      },
    },
    props: {
      image: String,
      name: String,
      surname: String,
    }
  }
</script>

<style lang="scss">
  @import '../assets/scss/colors.scss';
  $base-unit: calc(100vw / 48);
  .slide__player-container {
    display: grid;
    grid-column: 1 / 12;
    grid-template-columns: repeat(12, 1fr);
    width: 100vw;
    position: absolute;
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
    -webkit-text-stroke-width: 2px;
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
</style>
