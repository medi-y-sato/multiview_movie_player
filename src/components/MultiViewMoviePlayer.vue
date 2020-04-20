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

@Component
export default class MultiViewMoviePlayer extends Vue {
  play = false;
  videoUrlList: Array<string> = [];
  videoComponents: Array<HTMLVideoElement> = [];

  @Prop() private videopath1?: string;
  @Prop() private videopath2?: string;
  @Prop() private videopath3?: string;
  @Prop() private videopath4?: string;

  constructor() {
    super();

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
        videoComponent.src = url;
        videoComponent.className = "videoplayer";
        this.videoComponents[i] = videoComponent;
        console.log(`${i} : ${this.videoComponents[i].src}`);
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
        this.$nextTick(() => {
          videoColumn1.appendChild(videoComponent1);
        });
      }

      const videoColumn2 = document.querySelector(".col-video2");
      if (videoColumn2) {
        videoColumn2.childNodes.forEach(node => {
          node.remove();
        });
        videoComponent2.width = screenWidth * 0.9 * 0.2;
        this.$nextTick(() => {
          videoColumn2.appendChild(videoComponent2);
        });
      }
    });
  }

  changeVideo() {
    this.$nextTick(() => {
      const tempVideoComponent = this.videoComponents[0];
      this.videoComponents[0] = this.videoComponents[1];
      this.videoComponents[1] = tempVideoComponent;
      this.setController(this.videoComponents[0], this.videoComponents[1]);
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
