<template>
    <div>
        <video ref="videoClip" id="video" controls loop autoplay="true" playsinline="" preload="auto" tabindex="-1">
            <source :src="`https://actions.quarte-riposte.com${action.video_url}`" type="video/mp4">
        </video>

        <div class="row">
            <div class="col text-left">
                <b>{{ action.left_fencer_name }}</b>
            </div>
            <div class="col">
                <input type="checkbox" id="changeSpeed" v-model="playSlow" @click="changeSpeed()"/>
                <label id="changeSpeedLabel" for="changeSpeed">
                    Slow
                </label>
            </div>
            <div class="col text-right">
                <b>{{ action.right_fencer_name}}</b>
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'app',
    components: {},
    props: {
      action: {
        type: Object,
        default: undefined,
      },
    },
    data() {
      return {
        playSlow: false,
      };
    },
    watch: {
      action() {
        this.$refs.videoClip.load();
      },
    },
    methods: {
      changeSpeed() {
        const vid = this.$refs.videoClip;

        if (!this.playSlow) {
          vid.playbackRate = 0.65;
        } else {
          vid.playbackRate = 1;
        }
      },
      restart() {
        const vid = this.$refs.videoClip;
        vid.currentTime = 0;
      },
    },
  };
</script>

<style>
</style>
