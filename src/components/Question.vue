<template>
  <div class="question">
    <p class="question__title" v-html="question.question"></p>
    <div class="question__answers">
      <label
        class="question__answer"
        v-for="answer in question.answers"
        :key="answer"
      >
        <input
          type="radio"
          @change="$emit('change', answer)"
          :value="answer"
          :name="'Question-' + questionIndex + 1"
          :disabled="isAnswered"
        />
        <span v-html="answer" />
      </label>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    question: {
      type: Object,
      required: true,
    },
    questionIndex: {
      type: Number,
      required: true,
    },
    isAnswered: {
      type: Boolean,
      required: true,
    },
  },
};
</script>

<style>
.question {
  margin-bottom: 30px;
}

.question__title {
  font-weight: bold;
  margin-bottom: 20px;
  line-height: 26px;
}

.question__answers {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 16px;
}
</style>
