<template>
  <div class="card"
    :style="{ zIndex: zIndex }">
    <p>{{ text }}</p>
    <p>{{ index }}</p>
  </div>
</template>

<script>
export default {
  props: ['total', 'text', 'index'],
  data(){
    return {
      zIndex: this.total - this.index
    }
  }
}
</script>

<style>
  .card {
    position: absolute;
    height: 100%;
    width: 100%;
    border-radius: 15px;
    background-color: lightblue;
    cursor: move;
    cursor: grab;
    transform-style: preserve-3d;
    transform-origin: center 200%;
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
    font-size: 36px
  }
  .card:active {
    cursor: grabbing;
  }
</style>
