<template>
  <div class="cursor">
    <div class="cursor__circle" ref="cursorCircle"></div>
    <div class="cursor__dot" ref="cursorDot"></div>
  </div>
</template>

<script>
  import { gsap } from "gsap";

  export default {
    data() {
      return {
        x: null,
        y: null,
        circlePosX: null,
        circlePosY: null,
        dotPosX: null,
        dotPosY: null,
      };
    },
    methods: {
      animateCursor () {
        gsap.fromTo('#circle', {
          strokeDashoffset:"0%", 
          rotation:-90,
        }, {
          strokeDashoffset:"100%",
          duration: 1,
          ease: 'Power4.easeInOut'
        }
        )
      },
      cursorCustom(e) {
        this.x = e.clientX;
        this.y = e.clientY;

        const circle = this.$refs.cursorCircle;
        this.circlePosX = this.x - circle.clientWidth / 2;
        this.circlePosY = this.y - circle.clientWidth / 2;

        const dot = this.$refs.cursorDot;
        this.dotPosX = this.x - dot.clientWidth / 2;
        this.dotPosY = this.y - dot.clientHeight / 2;

        circle.style.transform = `translate(${this.circlePosX}px,${this.circlePosY}px)`;
        dot.style.transform = `translate(${this.dotPosX}px,${this.dotPosY}px)`;
      }
    },
    mounted() {
      window.addEventListener("mousemove", this.cursorCustom);
    }
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/colors.scss';
  $ease: cubic-bezier(0.23, 1, 0.32, 1);
  .cursor {
    cursor: none;
    pointer-events: none;
    z-index: 1000;
  }
  .cursor__circle {
    position: fixed;
    cursor: none;
    top: 0;
    left: 0;
    width: 48px;
    height: 48px;
    border: 1px solid $color-cursor-circle;
    border-radius: 50%;
    transform: translate(-100%, -100%);
    transition: transform 0.4s $ease;
  }
  .cursor__dot {
    position: fixed;
    cursor: none;
    top: 1px;
    left: 1px;
    width: 5px;
    height: 5px;
    border-radius: 50%;
    background: var(--accent);
    transform: translate(-100%, -100%);
    transition: transform 0.3s $ease;
  }
</style>