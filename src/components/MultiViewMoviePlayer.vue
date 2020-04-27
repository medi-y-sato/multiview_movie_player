<template>
  <v-container>
    <v-row>
      <div class="col-video">
        <div class="col-video1">video1</div>
        <div class="col-video2" @click="changeVideo()">video2</div>
      </div>
    </v-row>
    <v-row>
      <div>
        <button @click="clickPlaybutton()">
          <v-icon>{{ btnstate }}</v-icon>
        </button>
      </div>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import HLS from "hls.js";

@Component([HLS])
export default class MultiViewMoviePlayer extends Vue {
  play = false;
  videoUrlList: Array<string> = [];
  videoComponents: Array<HTMLVideoElement> = [];
  hlsComponents: Array<HLS> = [];
  masterVolume = 0.1;
  activeVideoIndex: number;
  maxActiveVideoIndex: number;

  @Prop() private videopath1?: string;
  @Prop() private videopath2?: string;
  @Prop() private videopath3?: string;
  @Prop() private videopath4?: string;

  constructor() {
    super();

    this.activeVideoIndex = 0;
    this.maxActiveVideoIndex = 1;

    let i = 0;
    console.log("constructor");
    [
      this.videopath1,
      this.videopath2,
      this.videopath3,
      this.videopath4
    ].forEach(url => {
      if (typeof url == "string") {
        const videoComponent: HTMLVideoElement = document.createElement(
          "video"
        );
        if (HLS.isSupported()) {
          this.hlsComponents[i] = new HLS();
          this.hlsComponents[i].loadSource(url);
          this.hlsComponents[i].attachMedia(videoComponent);
        } else {
          videoComponent.src = url;
          videoComponent.className = "videoplayer";
        }
        videoComponent.volume = this.masterVolume;
        this.videoComponents[i] = videoComponent;
        console.dir(this.videoComponents[i]);
        i++;
      }
    });

    this.setController(this.videoComponents[0], this.videoComponents[1]);
  }

  get btnstate(): string {
    if (this.play == true) {
      return "mdi-play";
    } else {
      return "mdi-pause";
    }
  }

  setController(
    videoComponent1: HTMLVideoElement,
    videoComponent2: HTMLVideoElement
  ) {
    this.$nextTick(() => {
      const screenWidth = document.body.clientWidth;

      const videoColumn1 = document.querySelector(".col-video1");
      if (videoColumn1) {
        videoColumn1.childNodes.forEach(node => {
          node.remove();
        });
        videoComponent1.width = screenWidth * 0.9;
        videoComponent1.volume = this.masterVolume;

        this.$nextTick(() => {
          videoColumn1.appendChild(videoComponent1);
        });
      }

      const videoColumn2 = document.querySelector(".col-video2");
      if (videoColumn2) {
        videoColumn2.childNodes.forEach(node => {
          node.remove();
        });
        videoComponent2.volume = 0;
        videoComponent2.width = screenWidth * 0.9 * 0.2;

        this.$nextTick(() => {
          videoColumn2.appendChild(videoComponent2);
        });
      }
    });
  }

  changeVideo() {
    this.activeVideoIndex =
      this.activeVideoIndex < this.maxActiveVideoIndex
        ? this.activeVideoIndex + 1
        : 0;
    console.log(`activeVideoIndex : ${this.activeVideoIndex}`);

    const subVideoIndex = this.activeVideoIndex == 0 ? 1 : 0;

    this.$nextTick(() => {
      const mainVideoComponent = this.videoComponents[this.activeVideoIndex];
      const subVideoComponent = this.videoComponents[subVideoIndex];

      this.hlsComponents[this.activeVideoIndex].autoLevelCapping = -1;
      this.hlsComponents[this.activeVideoIndex].nextLevel = -1;

      this.hlsComponents[subVideoIndex].autoLevelCapping = 0;
      this.hlsComponents[subVideoIndex].nextLoadLevel = 0;
      this.hlsComponents[subVideoIndex].nextLevel = 0;

      this.setController(mainVideoComponent, subVideoComponent);
    });
  }

  clickPlaybutton() {
    if (this.play == true) {
      this.videoComponents.forEach(videoComponents => {
        videoComponents.pause();
      });
      this.play = false;
    } else {
      this.videoComponents.forEach(videoComponents => {
        videoComponents.play();
      });
      this.play = true;
    }
  }
}
</script>

<style scoped>
.col-video {
  position: relative;
  height: 90%;
  margin: 0;
  padding: 0;
  background-color: aqua;
}
.col-video1 {
  position: relative;
  z-index: 1;
  padding: 0;
  margin: 0;
  float: right;
  background-color: bisque;
}
.col-video2 {
  position: absolute;
  z-index: 2;
  padding: 0;
  margin: 0;
  bottom: 1em;
  right: 2em;
  background-color: coral;
}
</style>
