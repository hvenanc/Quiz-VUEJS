<template>
  <div>
    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount"/>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <template v-for="answer in this.answers" v-bind:key="answer">
        <input 
        :disabled="answerSubmit"
        type="radio" 
        name="options" 
        id="answer"
        :value="answer"
        v-model="this.chosenAnswer" />
        <label v-html="answer"></label><br />
      </template>
      <button v-if="!this.answerSubmit" type="submit" @click="this.submitAwswer()" class="send">Check</button>

      <section v-if="answerSubmit" class="result">
        <h4 v-if="this.chosenAnswer == this.correctAnswer">
          &#9989; Congratulations, the answer "{{ this.correctAnswer }}" is correct!
        </h4>
        <h4 v-else>
          &#10060; I'm sorry, you picked the wrong answer! The correct answer is: "{{ this.correctAnswer }}"
        </h4>
        <button type="submit" @click="this.getNewQuestion()" class="send">Next Question</button>
      </section>
    </template>
  </div>
</template>

<script>

  import ScoreBoard from './components/ScoreBoard.vue';
  
  export default {
    name: "App",
    components: {
      ScoreBoard
    },

  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmit: false,
      winCount: 0,
      loseCount: 0,
    };
  },

  computed: {
    answers() {
      let answers = [...this.incorrectAnswers];
      let randowNumber = Math.round(Math.random() * answers.length);
      answers.splice(randowNumber, 0, this.correctAnswer);
      return answers;
    },
  },

  methods: {
    submitAwswer() {
      if(!this.chosenAnswer) {
        alert("Select one answer");
      }
      else {
        this.answerSubmit = true;
        if(this.chosenAnswer == this.correctAnswer) {
          this.winCount++;
        }
        else {
          this.loseCount++;
        }
      }
    },
    getNewQuestion() {
      this.chosenAnswer = undefined;
      this.answerSubmit = false;
      this.question = undefined;
      const api =
      "https://opentdb.com/api.php?amount=10&category=21&difficulty=easy";
    this.axios.get(api).then((response) => {
      this.question = response.data.results[0].question;
      this.incorrectAnswers = response.data.results[0].incorrect_answers;
      this.correctAnswer = response.data.results[0].correct_answer;
    });
    }
  },

  created() {
    this.getNewQuestion();
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

}

h1 {
  margin-top: 40px;
}

input[type='radio']{
  margin: 12px 4px;
}

button.send {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;
}

section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;

  span {
    padding: 8px;
    font-weight: bold;
    border: 1px solid black;
  }
}
</style>
