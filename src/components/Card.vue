<template>
  <div class="card" ref="card"
    :style="{ zIndex: zIndex }"
    @mousedown="onmousedown"
    @mouseup="onmouseup"
    @mousemove="onmousemove"
    @transitionend="ontransitionend">
    <p>{{ text }}</p>
    <p>{{ index }}</p>
  </div>
</template>

<script>
export default {
  props: ['total', 'answers', 'index'],
  data(){
    return {
      grabcard: false,
      startX: 0,
      text: null,
      zIndex: this.total - this.index,
      moveDistance: 0,
      chosen: false
    }
  },
  // mounted(){
  //   document.addEventListener('mousedown', e => this.onmousedown(e))
  //   document.addEventListener('mouseup', e => this.onmouseup(e))
  //   document.addEventListener('mousemove', e => this.onmousemove(e))
  // },
  methods: {
    onmousedown(e){
      e.preventDefault()
      this.grabcard = true
      this.startX = e.clientX
    },
    onmouseup(){
      const card = this.$refs.card

      if (Math.abs(this.moveDistance) < 20) {
        card.classList.add('reset')
      }

      if (this.moveDistance >= 20) {
        card.classList.add('toRight')
        this.chosen = true
        this.$emit('cardChosen', this.index)
      } else if (this.moveDistance <= -20) {
        card.classList.add('toLeft')
        this.chosen = true
        this.$emit('cardChosen')
      }

      this.grabcard = false
      card.style.transform = ""

      setTimeout(() => {
        card.classList.remove("toLeft", "toRight", "reset")
      }, 300)
    },
    onmousemove(e){
      e.preventDefault()
      if (this.grabcard){
        if (this.startX !== e.pageX) {
          this.moveDistance = e.pageX - this.startX

          if (this.moveDistance < 0) {
            this.text = this.answers[0]
          } else {
            this.text = this.answers[1]
          }

          this.$refs.card.style.transform = "translateX(" + this.moveDistance + "px) rotate(" + this.moveDistance / 10 + "deg)"
        }
      }
    },
    ontransitionend(){
      if (this.chosen){
        this.$refs.card.style.display = "none"
      }
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
