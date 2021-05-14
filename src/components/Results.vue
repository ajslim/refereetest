<template>
  <div>
    <div class="row result">
      <div class="col-lg-12">
        <h4>Your matched the verified call {{ score }} / {{ total }}</h4>
      </div>
    </div>
    <div v-for="(action, index) in verifiedActions">
      <div class="row result">

        <div class="col-lg-4">
          <VideoClip :action="action"/>
        </div>
        <div class="col-lg-7">
          <div v-if="action.correctCall !== undefined">
            <div v-if="action.correct" class="h5 mb-2"><b-icon variant="success" icon="check-circle"></b-icon>Your call matched the verified call!</div>
            <div>You said: {{ voteToString(action.submittedVote) }}</div>
            <div v-if="!action.correct">
              <div>The verified call was {{ voteToString(action.correctCall) }}</div>
            </div>
            <div>This action is classified as a {{ difficulty(action.difficulty) }} call</div>
          </div>
        </div>
        <div class="col-lg-1">
          <div class="actionId">{{ action.id }}</div>
        </div>
      </div>
    </div>
    <div class="row result">
      <div class="col-lg-12">
        <h4>You classified {{ newClassified }} new actions!</h4>
      </div>
    </div>
    <div v-for="(action, index) in unverifiedActions">
      <div class="row result">

        <div class="col-lg-4">
          <VideoClip :action="action"/>
        </div>
        <div class="col-lg-7">
          <div>You said: {{ voteToString(action.submittedVote) }}</div>

          <div v-if="action.correctCall === undefined">
            This action has not yet been verified.
          </div>
        </div>
        <div class="col-lg-1">
          <div class="actionId">{{ action.id }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { BIcon, BIconCheckCircle } from 'bootstrap-vue';
import 'bootstrap-vue/dist/bootstrap-vue-icons.min.css';
import VideoClip from './VideoClip';


export default {
  name: 'app',
  components: {
    VideoClip,
    BIcon,
    BIconCheckCircle,
  },
  props: {
    results: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {};
  },
  computed: {
    verifiedActions() {
      if (this.results.actions === undefined) {
        return [];
      }

      return this.results.actions.filter(action => action.correctCall !== undefined);
    },
    unverifiedActions() {
      if (this.results.actions === undefined) {
        return [];
      }

      return this.results.actions.filter(action => action.correctCall === undefined);
    },
    score() {
      return this.results.easyCorrectCount + this.results.mediumCorrectCount + this.results.difficultCorrectCount;
    },
    total() {
      return this.results.easy_count + this.results.medium_count + this.results.difficult_count;
    },
    newClassified() {
      if (this.results.actions === undefined) {
        return 0;
      }

      return this.results.actions.length - this.total;
    },
  },
  methods: {
    voteToString(vote) {
      const actionNames = [
        'Attack',
        'Counter Attack',
        'Riposte',
        'Remise',
        'Line',
        'Unknown / Other',
        'Simultaneous',
      ];

      const sides = [
        'Left',
        'Right',
      ];

      if (vote.call_id === 6) {
        return 'Unknown / Other';
      }

      if (vote.call_id === 7) {
        return 'Simultaneous';
      }

      return `${actionNames[parseInt(vote.call_id, 10) - 1]} from the ${sides[parseInt(vote.priority, 10) - 1]}`;
    },
    difficulty(difficulty) {
      const difficulties = [
        'Easy',
        'Medium',
        'Difficult',
      ];

      return difficulties[parseInt(difficulty, 10) - 1];
    },
  },
};
</script>

<style>
  .result {
    border-bottom: 1px solid #666666;
    padding-bottom: 15px;
    margin-bottom: 15px;
  }

  .actionId {
    color: #999999;
  }
</style>
