<template>
  <div>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <template v-for="answer in this.answers" v-bind:key="answer">
        <input 
        type="radio" 
        name="options" 
        id="answer"
        :value="answer"
        v-model="this.chosenAnswer" />
        <label v-html="answer"></label><br />
      </template>
      <button type="submit" @click="this.submitAwswer()">Check</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined
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

  created() {
    const api =
      "https://opentdb.com/api.php?amount=10&category=21&difficulty=easy";
    this.axios.get(api).then((response) => {
      this.question = response.data.results[0].question;
      this.incorrectAnswers = response.data.results[0].incorrect_answers;
      this.correctAnswer = response.data.results[0].correct_answer;
    });
  },
  methods: {
    submitAwswer() {
      if(!this.chosenAnswer) {
        alert("Select one answer");
      }
      else {
        if(this.chosenAnswer == this.correctAnswer) {
          alert("Correct")
        }
        else {
          alert("You lose!")
        }
      }
    }
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
  margin-top: 60px;
}
</style>
