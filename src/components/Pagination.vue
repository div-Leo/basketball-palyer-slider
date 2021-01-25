<template>
  <div class="slide__pagination">
    <div 
      v-for="page in pages" 
      @click="goToSlide(page)"
      :key="page" 
      class="slide__pagination-line"
      :class="{'slide__pagination-line--current': page === current}">
    </div>
  </div>
</template>

<script>
  export default {
    methods: {
      goToSlide (i) {
        if (this.current === i) return;
        this.$emit('chage-slide', i);
      },
    }, 
    props: {
      current: Number,
      pages: Array
    },
  }
</script>

<style lang="scss">
  @import '../assets/scss/colors.scss';
  $base-unit: calc(100vw / 48);

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
</style>
