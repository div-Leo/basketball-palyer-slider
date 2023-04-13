<template>
    <div class="cursor" >
      <div class="cursor__circle" ref="cursorCircle">
        <svg class="cursor__progress" viewBox="0 0 106 106" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
          <g id="progress" stroke="none" stroke-width="4" fill="none" fill-rule="evenodd">
            <g id="progressBar" transform="translate(-17.000000, -17.000000)">
              <circle class="cursor__progress-circle" stroke-width="1" fill-rule="nonzero" cx="70" cy="70" r="50"></circle>
              <transition @enter="enterCursor" @leave="leaveCursor">
                <path :key="current" class="cursor__progress-path" stroke-dasharray="0" id="circle" stroke-width="4" d="M70,120 C97.6142375,120 120,97.6142375 120,70 C120,42.3857625 97.6142375,20 70,20 C42.3857625,20 20,42.3857625 20,70 C20,97.6142375 42.3857625,120 70,120 Z" fill-rule="nonzero" transform="translate(70.000000, 70.000000) rotate(-125.000000) translate(-70.000000, -70.000000) "></path>
              </transition>
            </g>
          </g>
        </svg>
      </div>
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
          strokeDasharray: 0,
        }, {
          strokeDasharray:(100 * Math.PI) + ',9999',
          duration: 5,
          ease: 'linear'
        })
      },
      leaveCursor (el, done) {
        gsap.to(el, {
          strokeDasharray:0,
          duration: 0,
          ease: 'linear',
          onComplete: done,
        });
      },
      enterCursor (el, done) {
        gsap.to(el, {
          strokeDasharray: (100 * Math.PI) + ',9999',
          duration: 5,
          ease: 'linear',
          onComplete: done,
        });
      },
      cursorCustom(e) {
        this.x = e.clientX;
        this.y = e.clientY;

        const circle = this.$refs.cursorCircle;
        if (!circle) return;
        this.circlePosX = this.x - circle.clientWidth / 2;
        this.circlePosY = this.y - circle.clientWidth / 2;

        const dot = this.$refs.cursorDot;
        if (!dot) return;
        this.dotPosX = this.x - dot.clientWidth / 2;
        this.dotPosY = this.y - dot.clientHeight / 2;
        circle.style.transform = `translate(${this.circlePosX}px,${this.circlePosY}px)`;
        dot.style.transform = `translate(${this.dotPosX}px,${this.dotPosY}px)`;
      }
    },
    computed: {
      times () {
        return this.current;
      }
    },
    props : {
      current: Number
    },
    mounted() {
      this.animateCursor();
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
    transform: translate(-100%, -100%);
    transition: transform 0.4s $ease;
  }
  .cursor__dot {
    position: fixed;
    cursor: none;
    top: 0;
    left: 0;
    width: 5px;
    height: 5px;
    border-radius: 50%;
    background: var(--accent);
    transform: translate(-100%, -100%);
    /* transition: transform 0.3s $ease; */
  }
  .cursor__progress {
    height:100%;
    width:100%;
    transform: scaleX(-1) rotate(-55deg);
  }
  .cursor__progress-circle {
    stroke: $color-cursor-circle;
  }
  .cursor__progress-path {
    stroke: var(--accent);
  }
    
</style>