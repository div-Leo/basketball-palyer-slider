<template>
  <transition @enter="enterDetails">
    <div ref="details" :key="current" class="slide__player-details">
      <h4>Carrer Averages</h4>
      <br/>
        <p>
          points {{averages.points}}, 
          rebounds {{averages.rebounds}}, 
          assists {{averages.assists}}
        </p>
      <br/>
      <h4>Carrer Details</h4>
      <br/>
        <p>MWP award {{details.MWP}}</p>
        <p>{{details.teams.join(', ')}}</p>
        <p>Championships {{details.championships}}</p>
      <br/>
      <button class="slide__discover">
        DISCOVER MORE
        <svg xmlns="http://www.w3.org/2000/svg" width="15.427" height="8.422" viewBox="0 0 15.427 8.422">
          <g id="Arrow" transform="translate(0 0.918)">
            <line id="Line_16" data-name="Line 16" x2="14.01" transform="translate(0 3.318)" fill="none" stroke="#fff" stroke-width="1"/>
            <path id="Path_5" data-name="Path 5" d="M0,0A7.29,7.29,0,0,0,2.34.463,7.419,7.419,0,0,0,4.692,0a6.231,6.231,0,0,0-.53,2.286,6.432,6.432,0,0,0,.53,2.31" transform="translate(11.182) rotate(45)" fill="none" stroke="#fff" stroke-linecap="square" stroke-linejoin="bevel" stroke-width="1"/>
          </g>
        </svg>
      </button>
    </div>
  </transition>
</template>

<script> 
  import { gsap, TweenLite } from "gsap";

  export default {
    props: {
      current: Number,
      averages: {
        points: String,
        rebounds: String,
        assists: String,
      },
      details: {
        MWP: String,
        teams: String,
        championships: String,
      },
    },
    methods: {
      enterDetails () {
        gsap.from('p', {
          autoAlpha:0, 
          x:'20', 
          duration:.4,
          stagger: 0.1
        });
      }
    },
    mounted() {
      const { details } = this.$refs;

      let delay = 0;
      details.childNodes.forEach(el => {
        TweenLite.from(el, {
          autoAlpha:0, 
          x:'20', 
          duration: .6,
          delay: delay+=.07
        });
      })

      setInterval(() => {
        this.nextSlide();
      }, 10000);
    },
  }
</script>

<style lang="scss">
  .slide__player-details {
    font-size: 12px;
    grid-column: 10 / 12;
    text-align: right;
    line-height: 20px;
    margin-top: 50vh;
  }
  .slide__discover {
    font-family: "Saol Display";
    svg {
      margin-left: 5px;
    }
  }
</style>