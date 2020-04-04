<template>
    <div>
        <div class="vote-form">
            <div v-show="currentStepIndex === CALL_IT_FOR">
                <h6 class="text-muted">Call it for</h6>
                <div class="row">
                    <div class="col">
                        <input id="priority-left" name="priority" type="radio" value="1" v-model="vote.priority">
                        <label for="priority-left" class="text-center" @click="currentStepIndex = SELECT_ACTION">
                            Left
                        </label>
                    </div>

                    <div class="col text-center">
                        <input id="priority-right" name="priority" type="radio" value="2" v-model="vote.priority">
                        <label for="priority-right" class="text-center" @click="currentStepIndex = SELECT_ACTION">
                            Right
                        </label>
                    </div>
                </div>
                <div class="row">
                    <div class="col text-center">
                        <input id="priority-none" name="priority" type="radio" value="0" v-model="vote.priority">
                        <label for="priority-none" class="text-center" @click="currentStepIndex = SELECT_NEITHER">
                            - Neither -
                        </label>
                    </div>
                </div>
                <div class="row">
                    <div class="col text-center">
                        <input type="radio">
                        <label class="text-center" @click="currentStepIndex = SELECT_CARD">
                            Card
                        </label>
                    </div>
                </div>
            </div>
            <div class="calls" v-show="currentStepIndex === SELECT_ACTION">
                <div class="row">
                    <div class="col">
                        <input type="radio"/>
                        <label @click="currentStepIndex = CALL_IT_FOR">
                            Back
                        </label>
                    </div>
                </div>
                <h6 class="text-muted">Call</h6>
                <div class="row">
                    <div class="col">
                        <input id="call-attack" name="call" type="radio" value="1" v-model="vote.call_id">
                        <label for="call-attack" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Attack
                        </label>
                    </div>
                    <div class="col">
                        <input id="call-counter-attack" name="call" type="radio" value="2" v-model="vote.call_id">
                        <label for="call-counter-attack" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Counter-Attack
                        </label>
                    </div>
                </div>
                <div class="row">
                    <div class="col">
                        <input id="call-riposte" name="call" type="radio" value="3" v-model="vote.call_id">
                        <label for="call-riposte" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Riposte
                        </label>
                    </div>
                    <div class="col">
                        <input id="call-remise" name="call" type="radio" value="4" v-model="vote.call_id">
                        <label for="call-remise" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Remise
                        </label>
                    </div>
                </div>
                <div class="row">
                    <div class="col">
                        <input id="call-line" name="call" type="radio" value="5" v-model="vote.call_id">
                        <label for="call-line" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Line
                        </label>
                    </div>
                    <div class="col">
                        <input id="call-other" name="call" type="radio" value="6" v-model="vote.call_id">
                        <label for="call-other" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Unknown / Other
                        </label>
                    </div>
                </div>
                <div class="row">
                    <div class="col">
                        <input id="call-simultaneous" name="call" type="radio" value="7" v-model="vote.call_id">
                        <label for="call-simultaneous" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Simultaneous
                        </label>
                    </div>
                    <div class="col">
                    </div>
                </div>
            </div>

            <div class="calls" v-show="currentStepIndex === SELECT_NEITHER">
                <div class="row">
                    <div class="col">
                        <input type="radio"/>
                        <label @click="currentStepIndex = CALL_IT_FOR">
                            Back
                        </label>
                    </div>
                </div>
                <h6 class="text-muted">Call</h6>

                <div class="row">
                    <div class="col">
                        <input id="call-other2" name="call" type="radio" value="6" v-model="vote.call_id">
                        <label for="call-other2" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Unknown / Other
                        </label>
                    </div>

                    <div class="col">
                        <input id="call-simultaneous2" name="call" type="radio" value="7" v-model="vote.call_id">
                        <label for="call-simultaneous2" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Simultaneous
                        </label>
                    </div>
                </div>
                <hr>
                <h6 class="text-muted">Other</h6>
                <div class="row">
                    <div class="col">
                        <select name="vote-comment" @change="currentStepIndex = SUBMIT" v-model="vote.vote_comment_id">
                            <option value="1">No comment</option>
                            <option value="2">Not an action (clip doesn't have a halt, fencers only
                                testing, etc.)
                            </option>
                            <option value="3">Fencers listed on wrong sides</option>
                            <option value="4">Incorrect fencers listed</option>
                            <option value="5">Incorect bout details</option>
                        </select>
                    </div>
                </div>
            </div>

            <div class="cards" v-show="currentStepIndex === SELECT_CARD">
                <div class="row">
                    <div class="col">
                        <input type="radio"/>
                        <label @click="currentStepIndex = CALL_IT_FOR">
                            Back
                        </label>
                    </div>
                </div>
                <h6 class="text-muted">Card For</h6>

                <div class="row">
                    <div class="col">
                        <input id="card-left" name="card-for" type="radio" value="1" v-model="vote.card_for">
                        <label for="card-left" class="text-center" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Left
                        </label>
                    </div>

                    <div class="col text-center">
                        <input id="card-right" name="card-for" type="radio" value="2" v-model="vote.card_for">
                        <label for="card-right" class="text-center" @click="currentStepIndex = SELECT_DIFFICULTY">
                            Right
                        </label>
                    </div>
                </div>
            </div>

            <div v-show="currentStepIndex === SELECT_DIFFICULTY">
                <div class="row">
                    <div class="col">
                        <input type="radio"/>
                        <label @click="currentStepIndex = CALL_IT_FOR">
                            Back
                        </label>
                    </div>
                </div>
                <h6 class="text-muted">Difficulty / Certainty</h6>

                <div class="row">
                    <div class="col text-center">
                        <input id="difficulty-easy" name="difficulty" type="radio" value="1" v-model="vote.difficulty">
                        <label for="difficulty-easy" class="test" @click="currentStepIndex = SUBMIT">
                            Easy
                        </label>

                    </div>
                    <div class="col text-center">
                        <input id="difficulty-medium" name="difficulty" type="radio" value="2" v-model="vote.difficulty">
                        <label for="difficulty-medium"  @click="currentStepIndex = SUBMIT">
                            Medium
                        </label>
                    </div>
                    <div class="col text-center">
                        <input id="difficulty-hard" name="difficulty" type="radio" value="3" v-model="vote.difficulty">
                        <label for="difficulty-hard" @click="currentStepIndex = SUBMIT">
                            Hard
                        </label>
                    </div>
                </div>
            </div>

            <div v-show="currentStepIndex === SUBMIT">
                <div class="row">
                    <div class="col">
                        <input type="radio"/>
                        <label @click="currentStepIndex = CALL_IT_FOR">
                            Back
                        </label>
                    </div>
                </div>
                <label class="submit" @click="submit()">
                    Submit
                </label>
            </div>
        </div>
    </div>
</template>

<script>
  import 'bootstrap/dist/css/bootstrap.css'
  import 'bootstrap-vue/dist/bootstrap-vue.css'

  export default {
    name: 'app',
    components: {},
    props: {
      action: {
        type: Object,
        default: undefined,
      },
    },
    data: () => {

      const CALL_IT_FOR = 0;
      const SELECT_ACTION = 1;
      const SELECT_CARD = 2;
      const SELECT_DIFFICULTY = 3;
      const SELECT_NEITHER = 4;
      const OTHER = 5;
      const SUBMIT = 6;

      return {
        CALL_IT_FOR,
        SELECT_ACTION,
        SELECT_CARD,
        SELECT_DIFFICULTY,
        SELECT_NEITHER,
        OTHER,
        SUBMIT,
        currentStepIndex: CALL_IT_FOR,
        vote: {},
      }
    },
    methods: {
      submit() {
        this.action.vote = this.vote;
        this.vote = {};
        this.$emit('submit');
        this.currentStepIndex = this.CALL_IT_FOR;
      }
    }
  };
</script>

<style>
</style>
