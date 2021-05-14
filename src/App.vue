<template>
    <div id="app">
        <nav class="qrnav navbar navbar-expand-lg navbar-dark">
            <div class="container-fluid">
                <a class="navbar-brand" href="/">
                    <img src="./assets/quarte-riposte-logos-512x512-clear.png" height="80"/>
                    Actions
                </a>
            </div>
        </nav>

      <div v-if="showInstructions && actions.length > 0" class="container-fluid instructions">
        <div class="col-lg-12">
          <h3>You will be given {{ actions.length }} clips to referee</h3>

          <p>Some notes on making calls:</p>
          <ul>
            <li>Call it for the fencer who gets the touch, whether on target or off target</li>
            <li>A stop-hit should be given as a counter attack</li>
            <li>An attack from the left that falls short followed by an attack from the right should be given as an attack</li>
            <li>An attack in preparation should be given as an attack</li>
            <li>A simultaneous action with one light should be given as simultaneous</li>
            <li>If a fencer should be carded, select card and assign it to the correct fencer</li>
            <li>If there is a halt for other reasons, give it to neither and set the call as 'Other / Unknown'</li>
          </ul>

          <p>The difficulty of the call is determined by the consensus of the votes. If everyone makes the same call, it's easy. If everyone makes a different call, then it's difficult.</p>

          <p>There are some non-action clips in there too, such as the fencers testing their weapons. If that happens, set the call to neither, and select the appropriate comment from the drop down</p>

          <div class="col-lg-12">
            <label class="submit" @click="startTest()">
              Start test
            </label>
          </div>
        </div>
      </div>

        <div v-if="showResults && testResults !== undefined" class="container-fluid">
           <results :results="testResults"></results>

          <div class="row">
            <div class="col-lg-6">
              <label class="submit" @click="restart()">
                Again?
              </label>
            </div>
          </div>
        </div>

        <div v-if="showTest === true && actions !== undefined"  class="container-fluid">
            <div class="row">
                <div class="col-lg-6">
                    <div v-for="(videoAction, index) in actions">
                        <VideoClip ref="'video' + index" :action="videoAction" v-show="index === currentActionIndex" />
                    </div>

                </div>
                <div class="col-lg-6">
                    <VotingForm :action="action" @complete="nextAction()" />
                </div>

            </div>
        </div>

    </div>
</template>

<script>
  import 'bootstrap/dist/css/bootstrap.css';
  import 'bootstrap-vue/dist/bootstrap-vue.css';
  import 'bootstrap-vue/dist/bootstrap-vue-icons.min.css';
  import './assets/styles.less';


  import VotingForm from './components/VotingForm.vue';
  import VideoClip from './components/VideoClip.vue';
  import Results from './components/Results.vue';

  const apiBaseUrl = '/api';


  export default {
    name: 'app',
    components: {
      VotingForm,
      VideoClip,
      Results,
    },
    data() {
      return {
        actions: [],
        action: {},
        currentActionIndex: 0,
        playSlow: false,
        testId: undefined,
        showTest: false,
        showResults: false,
        showInstructions: true,
        testResults: {},
      };
    },
    async beforeMount() {
      this.reset();
    },
    methods: {
      startTest() {
        this.showTest = true;
        this.showInstructions = false;
      },
      async reset() {
        this.currentActionIndex = 0;
        const testResponse = await fetch(`${apiBaseUrl}/test/`);
        const test = await testResponse.json();
        this.testId = test.id;
        this.actions = test.actions;

        this.showResults = false;
        this.testResults = {};

        this.action = this.actions[this.currentActionIndex];
        this.action.index = this.currentActionIndex;
      },
      async restart() {
        this.reset();
        this.showTest = true;
      },
      nextAction() {
        this.showInstructions = false;
        this.currentActionIndex += 1;

        // Restart the clip at the beginning
        const video = this.$refs[`video${this.currentActionIndex}`];
        if (video !== undefined) {
          video.restart();
        }


        if (this.currentActionIndex >= this.actions.length) {
          this.getTestResults();
        } else {
          this.action = this.actions[this.currentActionIndex];
          this.action.index = this.currentActionIndex;
        }
      },
      async getTestResults() {
        const testResponse = await fetch(`${apiBaseUrl}/test/`, {
          method: 'POST',
          body: JSON.stringify({
            testId: this.testId,
            actions: this.actions,
          }),
        });

        this.showTest = false;
        this.showResults = true;

        this.testResults = await testResponse.json();
      },
    },
  };
</script>

<style>
  .instructions {
    padding-bottom: 15px;
    border-bottom: 1px solid #666666;
    margin-bottom: 15px;
  }
</style>
