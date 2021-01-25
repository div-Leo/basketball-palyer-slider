<template>
  <div ref="background" class="slide__background"/>
</template>

<script>
  import { gsap } from "gsap";

  export default {
    methods: {
      animateOnLoad () {
        const { background  } = this.$refs
        gsap.from(background,{
            height: 0,
            duration: 1,
            delay: .3,
            ease: 'power3.easeOut',
          },
        );
      },
    }, 
    mounted() {
      this.animateOnLoad();
    },
  }
</script>

<style lang="scss">
  @import '../assets/scss/colors.scss';

  .slide__background {
    position: absolute;
    grid-column: 1 / 6;
    height: 100%;
    width: 100%;
    background: var(--background);
    transition: background 1s ease;
  }
</style>
