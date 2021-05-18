<template>
    <div class="row">

      <div class="col-lg-6">
        <video v-if="randomClip.gfycat_gfy_id !== undefined" ref="videoClip" id="video" controls loop autoplay="true" playsinline="" preload="auto" tabindex="-1">
          <source :src="`https://thumbs.gfycat.com/${randomClip.gfycat_gfy_id}-mobile.mp4`" type="video/mp4">
        </video>
      </div>

      <div class="col-lg-6">
        <h3>Help out the Fencing Database</h3>
        <hr>
        <div v-for="(question, index) in questions">
          <div class="row">
            <div class="col-12">
              <h5>{{ question.question }}</h5>
            </div>
            <div class="col-12">
              {{ question.note }}
            </div>
            <div class="col-lg-4 col-6" v-for="option in question.options">
              <input :id="option.value" :name="question.id" type="radio" :value="option.value" v-model="question.answer">
              <label :for="option.value">
                {{ option.display }}
              </label>
            </div>
          </div>
          <hr>
        </div>
        <div class="row">
          <div class="col-lg-6">
            <label class="submit" @click="complete">
              Submit
            </label>
          </div>
        </div>
      </div>


    </div>
</template>

<script>
  import 'bootstrap/dist/css/bootstrap.css';
  import 'bootstrap-vue/dist/bootstrap-vue.css';

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
      const API_KEY = 'ndn7SW/H2lf+4iTduFmNXw==';

      return {
        randomClip: {},
        questions: {}
      }
    },
    methods: {
      async reset() {
        const API_KEY = 'ndn7SW/H2lf+4iTduFmNXw==';
        const questionsResponse = await fetch(`https://refereetest.quarte-riposte.com/fencingdbproxy/clips/questions/foil`);
        this.questions = await questionsResponse.json();

        const randomClipResponse = await fetch(`https://refereetest.quarte-riposte.com/fencingdbproxy/clips/random/foil`);
        this.randomClip = await randomClipResponse.json();

        this.$refs.videoClip.load();
      },
      async complete() {
        const submit = {};
        submit['gfycat-gfy-id'] = this.randomClip.gfycat_gfy_id;
        this.questions.forEach((question) => {
          submit[question.id] = question.answer;
        });
        await fetch(`https://refereetest.quarte-riposte.com/fencingdbproxy/clips/submit`, {
          method: 'POST',
          body: JSON.stringify(submit),
          headers: {
            "Content-type": "application/json; charset=UTF-8",
            "X-Authentication-Header":  'ndn7SW/H2lf+4iTduFmNXw==',
          }
        });

        this.$emit('submit');
      }
    }
  };
</script>

<style>
</style>
