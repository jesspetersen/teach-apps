<template>
  <div>
    <div id="questions-label" class="top-card">
      <h2>Questions</h2>
    </div>
    <div id="number-display" class="body-card">
      <div id="number-display-group">
        <span>Current: </span>
        <b-form-select id="number-selector" v-model="current" :options="GetNumberOfInputs" />
      </div>
    </div>
    <div id="answer-selector" class="bottom-card">
      <AnswerChoices :GetSelection="SelectedAnswer" :selected="answers[current - 1]"></AnswerChoices>
    </div>
    <br />
    <div id="how-to" class="top-card">
      <h2>How To</h2>
    </div>
    <div id="instructions" class="bottom-card">
      <h4>Selection of Correct Answers</h4>
      <span>
        The selection of Correct Answers screen allows you to input a set of correct answers for an assessment. This screen has the question number in a drop-down box at the top, and a column of five letters (from "A" to "E") in the centre of the page.
        <br />Start by clicking on the letter of the correct answer to the first question in you assessment. This letter must be from "A" to "E".
        As soon as you click on this letter, you will notice that the drop-down box labelled "Current" will change from "1" to "2". This means that you have successfully selected your first correct answer, and are now able to select the correct answer to the second question of your test.
        <br />Continue selecting correct answers until you have entered all of the answers for your assessment.
        <br />If you make a mistake, you can click on the drop-down menu next to "Current" in the top-centre of the screen and select the question number that you need to change. On this screen, the answer that you currently have chosen as the "correct" answer will be highlighted in red. To change this to a different answer, simply click on the circle that contains the new answer that you would like to select.
      </span>
      <h4>Narration of Answers</h4>
      <span>
        Once you have entered all of your correct answers, you can now ask the application to narrate them to you. Start by clicking on the "Play" button on the top bar at the top-centre of the screen. You can return to the "Selection" screen at any time by clicking on the "Play" button again.
        <br />To hear your answers narrated, you need to click on the "Play" button on the centre of the screen. Clicking the "Restart" button will make the narration start again from the beginning.
        <br /> You can use the slider to the left side of the "Play" and "Restart" buttons to adjust how fast the narrator will read out the correct answers.
      </span>
    </div>
  </div>
</template>

<style>
#number-display-group {
  margin: auto;
  font-size: 2em;
  padding: 0.2em;
  color: blueviolet;
}
#number-selector {
  border: 0.1em;
  border-style: solid;
  border-radius: 20%;
  border-color: blueviolet;
  font-size: 1.5em;
  color: blueviolet;
  height: 1.5em;
  width: 1.5em;
}
.testtest {
  height: 7em !important;
  background-color: aqua;
}
#number-display{
  background-color: white;
}
</style>

<script>
import AnswerChoices from './AnswerChoices.vue';

export default {
  name: "Answersinput",
  props: [
    "SetAllAnswers",
    "answers",
    ],
  components: {
    AnswerChoices,
  },
  data() {
    return {
      current: 1,
      selections: [],
      dropDownChoices: [],
    };
  },
  methods: {
    SelectedAnswer(e) {
      const value = e.target.getAttribute('value');
      if (this.answers.length >= this.current) {
        this.answers[this.current - 1] = value;
      } else {
        this.answers.push(value);
      }

      this.current += 1;
    },
  },
  computed: {
    GetNumberOfInputs() {
      var inputs = this.answers.map((_, ind) => {
        return ind + 1;
      });
      inputs.push(this.answers.length + 1);
      this.dropDownChoices = inputs;
      return this.dropDownChoices;
    },
  }
}
</script>
