<template>
  <div class="slide__number">
    <span class="slide__number-current">
      {{current+1}}
    </span>
    <span class="slide__number-total">
      /{{total}}
    </span>
  </div>
</template>

<script>
  export default {
    props: {
      current: Number,
      total: Number,
    },
  }
</script>

<style lang="scss">
  $base-unit: calc(100vw / 48);

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
</style>
