<template>
  <div>
    <br />
    <b-container>
      <b-row class="justify-content-md-center">
        <b-col col lg="1" v-for="x of selectLife()" :key="x">
          <b-icon-heart-fill
            v-if="life - x >= 0"
            style="color: #a2231d"
            scale="2"
            animation="cylon"
          ></b-icon-heart-fill>
          <b-icon-heart
            v-else
            rotate="15"
            scale="2"
            animation="fade"
          ></b-icon-heart>
        </b-col>
      </b-row>
    </b-container>
    <br />
    <br />
    <b-container fluid="lg">
      <b-row class="justify-content-lg-center">
        <b-col col lg="2">
          <b-icon-clock></b-icon-clock>
          <br />
          <b-button variant="outline-primary" disabled size="lg">
            {{
              String(time).slice(0, -2).length === 0
                ? "0"
                : String(time).slice(0, -2)
            }}
            , {{ String(time).slice(-2) }}
          </b-button>
        </b-col>
        <b-col col lg="2">
          <b-icon-clock></b-icon-clock>
          <b-icon-sort-numeric-down-alt></b-icon-sort-numeric-down-alt>
          <br />
          <b-button
            variant="outline-success"
            disabled
            size="lg"
            v-if="timerCount >= 3"
          >
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
    <b-modal
      v-model="gameOver"
      hide-header
      hide-footer
      size="sm"
      centered
    >
      <div class="text-center">
        <h1><b-icon-award color="brown"></b-icon-award> {{ counter }}</h1>
        <br />
        <h5><b-icon-check2 color="green"></b-icon-check2> {{ correct }}</h5>
        <h5>
          <b-icon-clock></b-icon-clock>
          {{
            String(time).slice(0, -2).length === 0
              ? "0"
              : String(time).slice(0, -2)
          }}
          , {{ String(time).slice(-2) }}
        </h5>
        <b-button
          class="mt-1"
          variant="outline-success"
          size="lg"
          block
          @click="newGame()"
          ><b-icon-arrow-counterclockwise></b-icon-arrow-counterclockwise
        ></b-button>
      </div>
    </b-modal>

    <br />
    <b-row class="justify-content-lg-center">
      <b-col>
        <h3>
          <b-icon-award color="brown"></b-icon-award> {{ counter }} /
          {{ correct }} <b-icon-check2 color="green"></b-icon-check2>
        </h3>
      </b-col>
    </b-row>
    <br />
  </div>
</template>

<script>
export default {
  name: "Home",
  props: ["level"],
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
    this.initData();
    this.getRandomNumber();
    this.countDownTimer();
    this.countUpTimer();
  },
  methods: {
    initData() {
      this.life = this.selectLife();
      this.timerCount = this.selectTimerCount();
    },
    newGame() {
      this.life = this.selectLife();
      this.timerCount = this.selectTimerCount();
      this.counter = 0;
      this.time = 0;
      this.correct = 0;
      this.gameOver = false;
      this.countUpTimer();
      this.countDownTimer();
    },
    nextOperation: function (answer) {
      if (answer == this.answer) {
        // Respuesta correcta
        this.correct = this.correct + 1;
        this.counter = this.counter + this.timerCount * 10;
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
      } else {
        this.randomNumber1 = this.generateNumber(this.min, this.max);
        this.randomNumber2 = this.generateNumber(this.min, this.max);
      }

      if (this.randomNumber1 >= this.randomNumber2) {
        this.question =
          this.randomNumber1 +
          this.arrayOperators[numberOperator] +
          this.randomNumber2;
        this.answer = this.calculateAnswer(
          this.randomNumber1,
          this.randomNumber2,
          numberOperator
        );
      } else {
        this.question =
          this.randomNumber2 +
          this.arrayOperators[numberOperator] +
          this.randomNumber1;
        this.answer = this.calculateAnswer(
          this.randomNumber2,
          this.randomNumber1,
          numberOperator
        );
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
      this.timerCount = this.selectTimerCount();
    },
    button2() {
      this.nextOperation(this.arrayAnswer[1]);
      this.timerCount = this.selectTimerCount();
    },
    button3() {
      this.nextOperation(this.arrayAnswer[2]);
      this.timerCount = this.selectTimerCount();
    },
    selectTimerCount() {
      let timerValue = 5;
      switch (this.level) {
        case 1:
          timerValue = 10;
          break;
        case 2:
          timerValue = 5;
          break;
        case 3:
          timerValue = 5;
          break;
        case 4:
          timerValue = 2;
          break;

        default:
          timerValue = 5;
          break;
      }
      return timerValue;
    },
    selectLife() {
      let lifes = 5;
      switch (this.level) {
        case 1:
          lifes = 5;
          break;
        case 2:
          lifes = 5;
          break;
        case 3:
          lifes = 3;
          break;
        case 4:
          lifes = 3;
          break;

        default:
          lifes = 3;
          break;
      }
      return lifes;
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
        } else {
          this.timerCount = this.selectTimerCount();
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
