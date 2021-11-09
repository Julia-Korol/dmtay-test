<template>
  <div id="app">
    <ol class="questions">
      <li v-for="(question, index) in questions" :key="question.question">
        <Question
          :question="question"
          :isAnswered="!!answeredQuestions[index]"
          :questionIndex="index"
          @change="selectAnswer(question, index, ...arguments)"
        />
      </li>
    </ol>

    <p class="result" v-if="isResultShown">
      <span class="result__title">Result: </span>
      {{ correctAnswers }}/{{ questions.length }}
    </p>
  </div>
</template>

<script>
import Question from "@/components/Question.vue";

export default {
  name: "App",
  components: { Question },
  data() {
    return {
      questions: [],
      answeredQuestions: {},
    };
  },
  computed: {
    isResultShown() {
      const questionsLength = this.questions.length;

      return (
        questionsLength &&
        Object.keys(this.answeredQuestions).length === questionsLength
      );
    },
    correctAnswers() {
      return Object.values(this.answeredQuestions).filter(
        ({ isCorrect }) => isCorrect
      ).length;
    },
  },
  methods: {
    async getQuestions() {
      const data = await fetch(
        "https://opentdb.com/api.php?amount=5&type=multiple"
      );
      const response = await data.json();

      this.questions = response.results.map(
        ({ question, correct_answer, incorrect_answers }) => ({
          question,
          correctAnswer: correct_answer,
          answers: [...incorrect_answers, correct_answer].sort(
            () => 0.5 - Math.random()
          ),
        })
      );
    },
    async selectAnswer({ question, correctAnswer }, index, answer) {
      await fetch("http://jsonplaceholder.typicode.com/posts", {
        method: "POST",
        body: JSON.stringify({
          question: question,
          answer: answer,
        }),
      });

      this.answeredQuestions = {
        ...this.answeredQuestions,
        [index]: { isCorrect: correctAnswer === answer },
      };
    },
  },
  mounted() {
    this.getQuestions();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.questions {
  max-width: 800px;
  margin: 40px auto;
}

.result {
  text-align: center;
  font-size: 20px;
  color: darkorange;
}

.result__title {
  font-weight: bold;
}
</style>
