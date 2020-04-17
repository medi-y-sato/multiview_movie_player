<template>
  <div class="MultiViewMoviePlayer">
    <video controls class="el-video">
      <source :src="videopath" />
    </video>
    <div>
      <button @click="clickPlaybutton()">{{btnstate}}</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class MultiViewMoviePlayer extends Vue {
  play = false;
  btnstate = "paused";

  @Prop() private videopath!: string;

  clickPlaybutton() {
    const videoComponent: Element = document.querySelector(".el-video")!;
    if (videoComponent instanceof HTMLMediaElement) {
      if (this.play == true) {
        videoComponent.pause();
        this.btnstate = "Paused";
        this.play = false;
      } else {
        videoComponent.play();
        this.btnstate = "Playing";
        this.play = true;
      }
    } else {
      throw new Error(".el-video is not video");
    }
  }
}
</script>

<style scoped>
</style>
