<template>
  <div>
    <div>
      <h2>Playback</h2>
      <h2>{{ ScreenDisplay() }}</h2>
      <b-progress :value="indexOfCurrent" :max="allAnswers.length -1"></b-progress>
      <input type="range" min="0.9  " max="1.5" step="0.05" v-model="rate">
      <b-button variant="primary" @click="play()" :disabled="this.atEnd">Play</b-button>
      <b-button variant="primary" @click="restart()">Restart</b-button>
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


<script>
export default {
  name: "AnswersPlayback",
  props: [
    "answers"
  ],
  data() {
    return {
      playing: false,
      allAnswers: [''],
      indexOfCurrent: 0,
      rate: 1.0,
      utterance: null,
      synth: null,
      atEnd: false,
    };
  },
  created() {
    this.allAnswers = this.allAnswers.concat(this.answers());
    this.utterance = new SpeechSynthesisUtterance();
    this.synth = window.speechSynthesis;
    
    this.utterance.onend = () => {
      this.SetNextSpoken();
      this.Speak(this.utterance);
    };
  },
  methods: {
    play() {
      this.playing = !this.playing;
      if (this.playing) {
        this.SetNextSpoken();
        this.Speak(this.utterance);
      }
    },
    restart() {
      this.playing = false;
      this.indexOfCurrent = 0;
      this.atEnd = false;
    },
    ScreenDisplay() {
      var question = this.indexOfCurrent;
      var answer = this.allAnswers[this.indexOfCurrent];
      return `${question} ${answer}`;
    },
    SetNextSpoken() {
      this.atEnd = !(this.indexOfCurrent < this.allAnswers.length - 1);
      this.playing = !this.atEnd && this.playing;
      if (this.playing) {
        this.indexOfCurrent += !this.atEnd ? 1 : 0;
        this.utterance.text = `${this.indexOfCurrent} ${this.allAnswers[this.indexOfCurrent]}`;
      }
    },
    Speak(utterance) {
      if (this.playing) {
        utterance.rate = this.rate;
        this.synth.speak(utterance);
      }
    },
  },
}
</script>

