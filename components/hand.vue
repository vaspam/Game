<template>
  <section>
    <div>
      <div>
        <div>
          <h1 v-show="tracker != 0">Saftey Points: {{tracker}}</h1>

          <!-- Shown after selecting a card -->
          <!-- ************ <p>Index of SelectedCard: {{selectedCard}}</p> -->
          <!-- Score *7 Cards in hand, after 7 cards score goes down* -->
          <h1>Score: {{score}}</h1>

          <!-- Add New Card -->
          <p>Add New Card</p>

          <!-- Generate Random Operation -->
          <p>Random Operation: {{operation}}</p>

          <!-- Top card in Pile -->
          <h1>Pile: {{firstCard}}</h1>
          <p>Add new top card to pile:</p>

          <!-- Choices -->
          <h1>Choice 1: {{num1}}</h1>
          <h1>Choice 2: {{num2}}</h1>
          <!-- Reset Choices -->
          <!-- Calculate Result of two number choices &  Random Operator -->
          <p>Total: {{total}}</p>

          <!-- -->
          <p>Hint: {{hint}}</p>
          <!-- Shows if Calculated Value equals to Top card in Pile -->
          <h1>Result: {{answer}}</h1>

          <h1>Test: {{num1}} {{num1Index}} | {{num2}} {{num2Index}}</h1>
        </div>
        <div>
          <button @click="addCard">Draw Card</button>
          <button @click="generateOperator">Generate Operator</button>
          <button @click="newTopCard">Cheat</button>
          <button @click="reset">Reset</button>
          <button @click="calculate">Calculate</button>
        </div>
      </div>
      <!-- Card Componenent -->
      <card
        v-for="cards in cardArray"
        :key="cards.id"
        :id="cards.id"
        :randomNum="cards.randomNum"
        @select="selectCard"
        :selectedCard="selectedCard"
      ></card>
    </div>
  </section>
</template>

<script>
import card from "~/components/card.vue";

export default {
  name: "hand",
  components: {
    card
  },
  created() {
    this.generateOperator();
    this.pushCard();
  },
  data() {
    return {
      //Main card Array
      cardArray: [],
      // Card Selector
      selectedCard: null,
      // Choices & calculated total
      num1: null,
      num1Index: null,
      num2: null,
      num2Index: null,
      total: "",
      // ***What's this for?
      cardIndex: 0,
      // Operation variable & Operation Randomizer | *** Random Operator has no initial value ***
      operation: "",
      randomOperator: 0,
      // Hint
      hint: "",
      // sets first card in pile
      firstCard: Math.floor(Math.random() * 20 + 1),
      // What's this for?
      answer: "",
      // Score
      score: "",
      tempScore: 0,
      // tracks if "saftey points" drop below 0
      tracker: 8,
      //controller for isSelected
      change: false,
      // Controller for switching between choice 1 and choice two
      choices: 0
    };
  },

  computed: {},
  methods: {
    /*Increments the value of cardIndex, tracker
     Decrements score if tracker drops below (0) 
     Adds (push's) items into array, 
     Gives randomNum prop a random num between 1 and 20
     Gives id prop new id(cardIndex) | Increments cardIndex
     */
    pushCard() {
      this.cardIndex += 1;
      this.cardArray.push({
        randomNum: Math.floor(Math.random() * 20 + 1),
        id: this.cardIndex
      });
    },
    addCard() {
      if (this.tracker === 0) {
        this.score -= 1;
        this.pushCard();
      } else {
        this.tracker -= 1;
        this.pushCard();
      }
    },
    /* gets values from cardArray (cards in cardArray)
      sets prop from cardArray (randomNum) to var num1 unless num1 already has a value
  */
    selectCard(cards) {
      this.selectedCard = cards.id;

      if (this.choices === 0) {
        this.num1 = cards.randomNum;
        this.num1Index = cards;
        this.choices = 1;
      } else {
        this.num2 = cards.randomNum;
        this.num2Index = cards;
        this.choices = 0;
      }
    },
    // resets choices
    reset() {
      this.num1 = null;
      this.num2 = null;
      this.total = null;
      this.num1Index = null;
      this.num2Index = null;
      this.hint = "";
    },
    // choose different card for pile
    newTopCard() {
      this.firstCard = Math.floor(Math.random() * 20 + 1);
      this.score -= 2;
    },
    // Generates and sets operation name by choosing a random number between 0 and 4
    generateOperator() {
      this.randomOperator = Math.floor(Math.random() * 4);

      switch (this.randomOperator) {
        case 0:
          this.operation = "Addition";
          this.tempScore = 10;
          break;
        case 1:
          this.operation = "Subtraction";
          this.tempScore = 10;
          break;
        case 2:
          this.operation = "Multiplication";
          this.tempScore = 20;
          break;
        case 3:
          this.operation = "Division";
          this.tempScore = 30;
          break;
        default:
          break;
      }
      this.addCard();
    },
    // switches between "randomOperation" to get its corresponding operation
    calculate(cards) {
      switch (this.randomOperator) {
        case 0:
          this.total = this.num1 + this.num2;
          if (this.total > 20) {
            cards.playable = false;
            this.hint = "Invalid Pair ";
          } else {
            this.hint = "Valid Pair";
          }
          break;
        case 1:
          this.total = Math.abs(this.num1 - this.num2);
          this.hint = "Valid Pair";
          break;
        case 2:
          this.total = this.num1 * this.num2;
          if (this.total > 20) {
            cards.playable = false;
            this.hint = "Invalid Pair";
          } else {
            this.hint = "Valid Pair";
          }
          break;
        case 3:
          var temp = 0;
          if (this.num1 > this.num2) {
            temp = Math.abs(this.num1 / this.num2);
          } else {
            temp = Math.abs(this.num2 / this.num1);
          }

          if (Number.isInteger(temp)) {
            this.total = temp;
            this.hint = "Valid Pair";
          } else {
            cards.playable = false;
            this.hint = "Invalid Pair";
          }
          break;
        default:
          break;
      }
      // checks if calculation of choices is equal to the card in pile
      // if not, adds new card
      if (this.total === this.firstCard) {
        this.firstCard = this.num2;
        this.score += this.tempScore;
        this.answer = "Success!";
        this.cardArray.splice(this.num2Index, 1);
        this.cardArray.splice(this.num1Index, 1);
        // Randomzie OPerator
        this.randomOperator = Math.floor(Math.random() * 4);
        // reset()
        this.reset();
        // Win Condition && Scenario
        if (this.score > 100) {
          alert("You Win!");
        }
      }
      // Lose Scenario
      else {
        this.answer = "No! ):";
        // addCard()
        this.addCard();
      }
    }
  }
};
</script>

<style scoped>
.green {
  color: #00C48D;
}
</style>
