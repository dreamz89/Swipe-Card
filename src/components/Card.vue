<template>
  <div class="card" :style="{ zIndex: zIndex }">
    <div class="innercard" :class="index === 0 ? 'is-flipped' : null">
      <div class="card__face card__face--front"></div>
      <div class="card__face card__face--back">
        <p>{{ answer }}</p>
        <img :src="photo"/>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['total', 'answer', 'photo', 'index'],
  data(){
    return {
      zIndex: this.total - this.index
    }
  }
}
</script>

<!-- https://3dtransforms.desandro.com/card-flip -->
<style>
  .card {
    position: absolute;
    height: 100%;
    width: 100%;
    border-radius: 15px;
    cursor: move;
    cursor: grab;
  }
  .card.reset {
    transition: transform 0.3s;
    transform: translateX(0) !important;
  }
  .card.toLeft {
    transition: transform 0.3s;
    transform: translateX(-300px) rotate(-15deg) !important;
  }
  .card.toRight {
    transition: transform 0.3s;
    transform: translateX(300px) rotate(15deg) !important;
  }
  .card p {
    font-size: 36px;
  }
  .card img {
    width: 80%;
  }
  .card:active {
    cursor: grabbing;
  }
  .innercard {
    height: 100%;
    width: 100%;
    transition: transform 1s;
    position: relative;
    transform-style: preserve-3d;
  }
  .card__face {
    position: absolute;
    height: 100%;
    width: 100%;
    border-radius: 15px;
    backface-visibility: hidden;
  }
  .card__face--front {
    background-color: lightblue;
  }
  .card__face--back {
    background-color: lightgreen;
    transform: rotateY(180deg);
  }
  .innercard.is-flipped {
    transform: rotateY(180deg);
  }
</style>
