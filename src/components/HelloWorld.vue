<template>
  <div>
    <p>{{ cardsInfo[cardNumber].question }}</p>
    <div class="game"
      @mousedown="onmousedown"
      @touchstart="onmousedown"
      @mousemove="onmousemove"
      @touchmove="onmousemove"
      @mouseup="onmouseup"
      @touchend="onmouseup"
      @transitionend="ontransitionend">
      <Card
        v-for="(card, index) in cardsInfo"
        :key="index"
        :total="cardsInfo.length"
        :text="text"
        :index="index"
      ></Card>
    </div>
  </div>
</template>

<script>
import Card from './Card.vue'
export default {
  components: { Card },
  data(){
    return {
      cardNumber: 0,
      cardsInfo: [{
        question: 'Should we do a Tinder-style swipe project?',
        answers: ['Yes', 'No']
      }, {
        question: 'Are you sure?',
        answers: ['Yes', 'No']
      }, {
        question: 'Double confirm?',
        answers: ['Yes', 'No']
      }],
      cardsArray: [],
      grabcard: false,
      startX: 0,
      text: null,
      moveDistance: 0,
      chosen: false
    }
  },
  mounted() {
    var cards = document.querySelectorAll('.card')
    this.cardsArray = Array.prototype.slice.call(cards)
  },
  methods: {
    onmousedown(e) {
      e.preventDefault()
      this.grabcard = true
      this.startX = e.pageX || e.touches[0].pageX
    },
    onmousemove(e) {
      e.preventDefault()
      const activeCard = this.cardsArray[this.cardNumber]

      if (this.grabcard){
        if (this.startX !== e.pageX) {
          this.moveDistance = (e.pageX || e.touches[0].pageX) - this.startX

          if (this.moveDistance < 0) {
            this.text = this.cardsInfo[this.cardNumber].answers[0]
          } else {
            this.text = this.cardsInfo[this.cardNumber].answers[1]
          }

          activeCard.style.transform = "translateX(" + this.moveDistance + "px) rotate(" + this.moveDistance / 10 + "deg)"
        }
      }
    },
    onmouseup() {
      const activeCard = this.cardsArray[this.cardNumber]

      if (Math.abs(this.moveDistance) < 20) {
        activeCard.classList.add('reset')
        this.text = null
      }

      if (this.moveDistance >= 20) {
        activeCard.classList.add('toRight')
        this.chosen = true
      } else if (this.moveDistance <= -20) {
        activeCard.classList.add('toLeft')
        this.chosen = true
      }

      this.grabcard = false
      activeCard.style.transform = ""

      setTimeout(() => {
        activeCard.classList.remove("toLeft", "toRight", "reset")
      }, 300)
    },
    ontransitionend(e) {
      if (this.chosen){
        this.cardsArray[this.cardNumber].style.display = "none"
        this.cardNumber < this.cardsInfo.length - 1 ? this.cardNumber += 1 : null
        const innercards = document.querySelectorAll('.innercard')
        Array.prototype.slice.call(innercards)[this.cardNumber].classList.add('is-flipped')
      }

      this.chosen = false
    }
  }
}
</script>

<style>
  .game {
    position: relative;
    height: 400px;
    width: 400px;
    margin: 0 auto;
    border-radius: 15px;
  }
</style>
