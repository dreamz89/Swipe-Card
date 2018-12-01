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
        :answer="answer"
        :photo="cardsInfo[index].photo"
        :explanation="cardsInfo[index].explanation"
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
        answers: ['Yes', 'No'],
        photo: 'art.jpg',
        explanation: null
      }, {
        question: 'Should we do a Tinder-style swipe project?',
        answers: null,
        photo: 'art.jpg',
        explanation: 'It is interactive for the user.'
      }, {
        question: 'Are you sure?',
        answers: ['Yes', 'No'],
        photo: 'art.jpg',
        explanation: null
      }, {
        question: 'Are you sure?',
        answers: null,
        photo: 'art.jpg',
        explanation: 'You must be sure'
      }, {
        question: 'Double confirm?',
        answers: ['Yes', 'No'],
        photo: 'art.jpg',
        explanation: null
      }, {
        question: 'Double confirm?',
        answers: null,
        photo: 'art.jpg',
        explanation: 'Okay let\'s do it!'
      }],
      cardsArray: [],
      grabcard: false,
      startX: 0,
      answer: 'Left or right',
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
            this.answer = this.cardsInfo[this.cardNumber].answers[0]
          } else {
            this.answer = this.cardsInfo[this.cardNumber].answers[1]
          }

          activeCard.style.transform = "translateX(" + this.moveDistance + "px) rotate(" + this.moveDistance / 10 + "deg)"
        }
      }
    },
    onmouseup() {
      const activeCard = this.cardsArray[this.cardNumber]

      if (Math.abs(this.moveDistance) < 20) {
        activeCard.classList.add('reset')
        this.answer = null
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
      }, 350)
    },
    ontransitionend() {
      if (this.chosen){
        this.cardsArray[this.cardNumber].style.display = "none"
        this.cardNumber < this.cardsInfo.length - 1 ? this.cardNumber += 1 : null
        this.answer = 'Left or right'
        const innercards = document.querySelectorAll('.innercard')
        Array.prototype.slice.call(innercards)[this.cardNumber].classList.add('is-flipped')
      }

      this.chosen = false
    }
  }
}
</script>

<style>
  body {
    margin: 0
  }
  .game {
    position: relative;
    height: 400px;
    max-width: 400px;
    width: 90%;
    margin: 0 auto;
    border-radius: 15px;
  }
</style>
