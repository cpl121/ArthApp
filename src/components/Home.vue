<template>
  <div>
    <br />
    <b-container fluid="sm">
      <b-row class="justify-content-md-center">
        <b-col col lg="1">
          <b-icon-heart-fill
            style="color: #a2231d"
            v-if="life > 1"
            scale="2"
            animation="cylon"
          ></b-icon-heart-fill>
          <b-icon-heart-fill
            style="color: #B33127"
            v-else-if="life == 1"
            scale="2"
            animation="throb"
          ></b-icon-heart-fill>
          <b-icon-heart rotate="15" scale="2" animation="fade" v-else></b-icon-heart>
        </b-col>
        <b-col col lg="1">
          <b-icon-heart-fill
            style="color: #a2231d"
            v-if="life >= 2"
            scale="2"
            animation="cylon"
          ></b-icon-heart-fill>
          <b-icon-heart rotate="15" scale="2" animation="fade" v-else></b-icon-heart>
        </b-col>
        <b-col col lg="1">
          <b-icon-heart-fill
            style="color: #a2231d"
            v-if="life >= 3"
            scale="2"
            animation="cylon"
          ></b-icon-heart-fill>
          <b-icon-heart rotate="15" scale="2" animation="fade" v-else></b-icon-heart>
        </b-col>
      </b-row>
    </b-container>
    <br />
    <br />
    <b-container fluid="lg">
      <b-row class="justify-content-lg-center">
        <b-col col lg="2">
          <b-icon-clock></b-icon-clock>
          <br>
          <b-button variant="outline-primary" disabled size="lg">
            {{ String(time).slice(0, -2).length === 0? "0" : String(time).slice(0, -2) }} 
            , {{ String(time).slice(-2) }}
            </b-button>
        </b-col>
        <b-col col lg="2">
          <b-icon-clock></b-icon-clock>
          <b-icon-sort-numeric-down-alt></b-icon-sort-numeric-down-alt>
          <br>
          <b-button variant="outline-success" disabled size="lg" v-if="timerCount >=3">
              {{ timerCount }}
            </b-button>
            <b-button variant="outline-danger" disabled size="lg" v-else>
              <b>
              {{ timerCount }}
              </b>
            </b-button>
        </b-col>
      </b-row>
    </b-container>
    <br />
    <p style="font-size: 40px">{{ question }}</p>
    <b-container fluid="sm">
      <b-row class="justify-content-md-center">
        <b-col col lg="1">
          <b-button
            class="button"
            size="lg"
            variant="outline-dark"
            @click="button1()"
            >{{ arrayAnswer[0] }}</b-button
          >
        </b-col>
        <b-col col lg="1">
          <b-button
            class="button"
            size="lg"
            variant="outline-dark"
            @click="button2()"
            >{{ arrayAnswer[1] }}</b-button
          >
        </b-col>
        <b-col col lg="1">
          <b-button
            class="button"
            size="lg"
            variant="outline-dark"
            @click="button3()"
            >{{ arrayAnswer[2] }}
          </b-button>
        </b-col>
      </b-row>
    </b-container>
    <b-modal v-model="gameOver" hide-footer size="sm" title="GAME OVER" hide-header centered>
      <div class="d-block text-center">
        <h1><b-icon-award color="brown"></b-icon-award> {{ counter }}</h1>
        <br />
        <h5><b-icon-check2 color="green"></b-icon-check2> {{ correct }}</h5>
        <h5><b-icon-clock></b-icon-clock>
        {{ String(time).slice(0, -2).length === 0? "0" : String(time).slice(0, -2) }} 
            , {{ String(time).slice(-2) }}</h5>
        <b-button class="mt-2" variant="outline-success" size="lg" block @click="newGame()"><b-icon-arrow-counterclockwise></b-icon-arrow-counterclockwise></b-button>
      </div>
    </b-modal>
    <br />
    <b-row class="justify-content-lg-center">
        <b-col>
          <h3><b-icon-award color="brown"></b-icon-award> {{ counter }} / {{ correct }} <b-icon-check2 color="green"></b-icon-check2></h3>
        </b-col>
      </b-row>
    <br />
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      life: 3,
      randomNumber1: 1,
      randomNumber2: 1,
      check: false,
      show: 0,
      question: "",
      answer: 0,
      min: 1,
      max: 99,
      counter: 0,
      correct: 0,
      arrayAnswer: [],
      arrayOperators: ["+", "-", "*"],
      gameOver: false,
      timerCount: 5,
      time: 0,
    };
  },
  created: function () {
    this.getRandomNumber();
    this.countDownTimer();
    this.countUpTimer();
  },
  watch: {},
  methods: {
    newGame() {
      this.life = this.life = 3;
      this.counter = 0;
      this.time = 0;
      this.timerCount = 5;
      this.correct = 0;
      this.gameOver = false;
      this.countUpTimer();
    },
    nextOperation: function (answer) {
      if (answer == this.answer) {
        // Respuesta correcta
        this.correct = this.correct + 1;
        this.counter = this.counter + (this.timerCount * 10);
        this.check = true;
        this.show = 2;
      } else {
        // Respuesta incorrecta
        this.check = false;
        this.show = 2;
        this.life = this.life - 1;
        if (this.life == 0) {
          // Fin de la partida
          this.gameOver = true;
        }
      }
      this.arrayAnswer = [];
      this.getRandomNumber();
    },
    getRandomNumber: function () {
      let numberOperator = this.generateNumber(0, 2);
      if (numberOperator === 2) {
        this.randomNumber1 = this.generateNumber(this.min, 10);
        this.randomNumber2 = this.generateNumber(this.min, 10);
      }
      else {
        this.randomNumber1 = this.generateNumber(this.min, this.max);
        this.randomNumber2 = this.generateNumber(this.min, this.max);
      }

      if (this.randomNumber1 >= this.randomNumber2) {
        this.question = this.randomNumber1 + this.arrayOperators[numberOperator] + this.randomNumber2;
        this.answer = this.calculateAnswer(this.randomNumber1, this.randomNumber2, numberOperator);
      } else {
        this.question = this.randomNumber2 + this.arrayOperators[numberOperator] + this.randomNumber1;
        this.answer = this.calculateAnswer(this.randomNumber2, this.randomNumber1, numberOperator);
      }
      this.arrayAnswer = this.checkEqualNumber(this.answer);
    },
    checkEqualNumber(answer) {
      let arrayFinal = [];
      let newNumber = this.generateNumber(this.answer - 6, this.answer + 6);
      while (newNumber == answer) {
        newNumber = this.generateNumber(this.answer - 7, this.answer + 7);
      }
      let newNumber2 = this.generateNumber(this.answer - 6, this.answer + 6);
      while (newNumber2 == answer || newNumber2 == newNumber) {
        newNumber2 = this.generateNumber(this.answer - 6, this.answer + 6);
      }
      arrayFinal.push(answer, newNumber, newNumber2);
      this.shuffleArray(arrayFinal);
      return arrayFinal;
    },
    generateNumber: function (min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    shuffleArray(arrayAnswer) {
      arrayAnswer.sort(() => Math.random() - 0.5);
    },
    calculateAnswer(number1, number2, operatorNumber) {
      let finalAnswer;
      switch (operatorNumber) {
        case 0:
          finalAnswer = number1 + number2;
          break;
        case 1:
          finalAnswer = number1 - number2;
          break;
        case 2:
          finalAnswer = number1 * number2;
          break;
      
        default:
          finalAnswer = number1 + number2;
          break;
      }
      return finalAnswer;
    },
    button1() {
      this.nextOperation(this.arrayAnswer[0]);
      this.timerCount = 5;
    },
    button2() {
      this.nextOperation(this.arrayAnswer[1]);
      this.timerCount = 5;
    },
    button3() {
      this.nextOperation(this.arrayAnswer[2]);
      this.timerCount = 5;
    },
    countDownTimer() {
      if (this.timerCount > 0) {
        setTimeout(() => {
          this.timerCount -= 1;
          this.countDownTimer();
        }, 1000);
      } else {
        this.life = this.life - 1;
        if (this.life === 0) {
          this.gameOver = true;
        }
        else {
          this.timerCount = 5;
          this.countDownTimer();
        }
      }
    },
    countUpTimer() {
      if (!this.gameOver) {
        setTimeout(() => {
          this.time += 10;
          this.countUpTimer();
        }, 100);
      }
    },
  },
};
</script>

<style scoped>
.button {
  background-color: #68b1c0;
  color: azure;
}

</style>
