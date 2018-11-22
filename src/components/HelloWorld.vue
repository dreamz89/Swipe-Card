<template>
  <div>
    <p>{{ cardsInfo[cardNumber].question }}</p>
    <div class="game">
      <Card
        v-for="(card, index) in cardsInfo"
        :key="index"
        :total="cardsInfo.length"
        :answers="card.answers"
        :index="index"
        @cardChosen="cardNumber < cardsInfo.length - 1 ? cardNumber += 1 : cardNumber"
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
      }]
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
