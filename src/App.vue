<template>
    <div id="app">
        <nav class="qrnav navbar navbar-expand-lg navbar-dark">
            <div class="container-fluid">
                <a class="navbar-brand" href="/">
                    <img src="./assets/quarte-riposte-logos-512x512-clear.png" height="80"/>
                    Actions
                </a>

                <div  @click="test">
                    Test
                </div>
            </div>
        </nav>

        <div  v-show="false">

        </div>

        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-6">
                    <div v-for="(videoAction, index) in actions">
                        <VideoClip :action="videoAction" v-show="index === currentActionIndex" />
                    </div>

                </div>
                <div class="col-lg-6">
                    <VotingForm :action="action" @submit="nextAction()" />
                </div>

            </div>
        </div>

    </div>
</template>

<script>
  import './assets/styles.less';
  import 'bootstrap/dist/css/bootstrap.css'
  import 'bootstrap-vue/dist/bootstrap-vue.css'

  import VotingForm from './components/VotingForm'
  import VideoClip from './components/VideoClip'

  const apiBaseUrl = '/api';


  export default {
    name: 'app',
    components: {
      VotingForm,
      VideoClip
    },
    data: () => {
      return {
        actions: [],
        action: {},
        currentActionIndex: 0,
        playSlow: false,
        testId: undefined,
      }
    },
    async beforeMount() {
      // const testResponse = await fetch(`${apiBaseUrl}/test/`);
      // const test = await testResponse.json();
      this.testId = 41;

      this.actions = [{
        "video_url": "/storage/bout/CU_kcezU0Eg/clips/57.mp4",
        "left_fencer_name": "SIDO, Alexandre",
        "right_fencer_name": "BRAVO, Kenji",
        "thumb_url": "/storage/bout/CU_kcezU0Eg/lightthumbs/57.png",
        "index": 0,
        "vote": {"priority": "1", "call_id": "1", "difficulty": "1"}
      }, {
        "video_url": "/storage/bout/9HE-fFeM83g/clips/218.mp4",
        "left_fencer_name": "CHEUNG, Ka Long",
        "right_fencer_name": "BIANCHI, Guillaume",
        "thumb_url": "/storage/bout/9HE-fFeM83g/lightthumbs/218.png",
        "index": 1,
        "vote": {"priority": "1", "call_id": "2", "difficulty": "1"}
      }, {
        "video_url": "/storage/bout/yXfD_F5beS4/clips/331.mp4",
        "left_fencer_name": "RIGIN, Dmitry",
        "right_fencer_name": "HA, Taegyu",
        "thumb_url": "/storage/bout/yXfD_F5beS4/lightthumbs/331.png",
        "index": 2,
        "vote": {"priority": "1", "call_id": "3", "difficulty": "2"}
      }, {
        "video_url": "/storage/bout/8bPjIKTM_R0/clips/1372.mp4",
        "left_fencer_name": "DOUGLAS, Sholto",
        "right_fencer_name": "DAVIS, James-Andrew",
        "thumb_url": "/storage/bout/8bPjIKTM_R0/lightthumbs/1372.png",
        "index": 3,
        "vote": {"priority": "1", "call_id": "5", "difficulty": "2"}
      }, {
        "video_url": "/storage/bout/NG8cgGQ6x9c/clips/417.mp4",
        "left_fencer_name": "DE GREEF, Stef",
        "right_fencer_name": "GAROZZO, Daniele",
        "thumb_url": "/storage/bout/NG8cgGQ6x9c/lightthumbs/417.png",
        "index": 4,
        "vote": {"priority": "1", "call_id": "3", "difficulty": "2"}
      }, {
        "video_url": "/storage/bout/QeHo5Qsuh1A/clips/1055.mp4",
        "left_fencer_name": "CHEREMISINOV, Alexey",
        "right_fencer_name": "MEPSTEAD, Marcus",
        "thumb_url": "/storage/bout/QeHo5Qsuh1A/lightthumbs/1055.png",
        "index": 5,
        "vote": {"priority": "1", "call_id": "2", "difficulty": "2"}
      }, {
        "video_url": "/storage/bout/8_yX1HRpDgk/clips/606.mp4",
        "left_fencer_name": "POGREBNIAK, Andrii",
        "right_fencer_name": "SAFIN, Timur",
        "thumb_url": "/storage/bout/8_yX1HRpDgk/lightthumbs/606.png",
        "index": 6,
        "vote": {"priority": "0", "call_id": "7", "difficulty": "1"}
      }, {
        "video_url": "/storage/bout/CD_OaN263kM/clips/471.mp4",
        "left_fencer_name": "MEINHARDT, Gerek",
        "right_fencer_name": "CHEREMISINOV, Alexey",
        "thumb_url": "/storage/bout/CD_OaN263kM/lightthumbs/471.png",
        "index": 7,
        "vote": {"priority": "0", "call_id": "7", "difficulty": "2"}
      }, {
        "video_url": "/storage/bout/z8oa8rj9B08/clips/536.mp4",
        "left_fencer_name": "CASSARA, Andrea",
        "right_fencer_name": "GAROZZO, Daniele",
        "thumb_url": "/storage/bout/z8oa8rj9B08/lightthumbs/536.png",
        "index": 8,
        "vote": {"priority": "1", "call_id": "3", "difficulty": "2"}
      }, {
        "video_url": "/storage/bout/fVrmBvz8n74/clips/1197.mp4",
        "left_fencer_name": "RIGIN, Dmitry",
        "right_fencer_name": "LE PECHOUX, Erwann",
        "thumb_url": "/storage/bout/fVrmBvz8n74/lightthumbs/1197.png",
        "index": 9,
        "vote": {"priority": "1", "call_id": "1", "difficulty": "1"}
      }];

      this.action = this.actions[this.currentActionIndex];
      this.action.index = this.currentActionIndex;
    },
    methods: {
      async nextAction() {
        this.currentActionIndex += 1;
        if (this.currentActionIndex >= this.actions.length) {

        } else {
          this.action = this.actions[this.currentActionIndex];
          this.action.index = this.currentActionIndex;
        }
      },
      async test() {
        console.log(this.actions);

        const testResponse = await fetch(`${apiBaseUrl}/test/`, {
          method: 'POST',
          body: JSON.stringify({
            testId: this.testId,
            actions: this.actions
          })
        });
        const testResults = await testResponse.json();
        console.log(testResults);
      }
    },
  };
</script>

<style>
</style>
