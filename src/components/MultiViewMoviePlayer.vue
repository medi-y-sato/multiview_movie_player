<template>
  <v-container>
    <v-row>
      <div class="col-video">
        <div class="col-video-main">
          <div class="col-video1"></div>
        </div>
        <div class="col-video-sub">
          <div class="col-video2" @click="changeVideo(1)"></div>
          <div class="col-video3" @click="changeVideo(2)"></div>
          <div class="col-video4" @click="changeVideo(3)"></div>
        </div>
      </div>
    </v-row>
    <v-row>
      <div class="playertools">
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
        if (/\.m3u8$/.test(url) && HLS.isSupported()) {
          this.hlsComponents[i] = new HLS();
          this.hlsComponents[i].loadSource(url);
          this.hlsComponents[i].attachMedia(videoComponent);
        } else {
          videoComponent.src = url;
          videoComponent.className = "videoplayer";
        }
        videoComponent.volume = 0;
        this.videoComponents[i] = videoComponent;
        console.dir(this.videoComponents[i]);
        i++;
      }
    });

    this.setController(
      this.videoComponents[0],
      this.videoComponents[1],
      this.videoComponents[2],
      this.videoComponents[3]
    );
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
    videoComponent2: HTMLVideoElement,
    videoComponent3: HTMLVideoElement,
    videoComponent4: HTMLVideoElement
  ) {
    this.$nextTick(() => {
      let screenWidth = 0;
      let screenHeight = 0;

      const videoDiv = document.querySelector(".col-video-main");
      if (videoDiv) {
        screenWidth = videoDiv.clientWidth;
        screenHeight = videoDiv.clientHeight;
      }

      const videoColumn1 = document.querySelector(".col-video1");
      if (videoColumn1) {
        videoColumn1.childNodes.forEach(node => {
          node.remove();
        });
        videoComponent1.volume = this.masterVolume;
        videoComponent1.width = screenWidth;
        videoComponent1.height = screenHeight;

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
        videoComponent2.width = screenWidth / 8;
        videoComponent2.height = screenHeight / 8;

        this.$nextTick(() => {
          videoColumn2.appendChild(videoComponent2);
        });
      }
      const videoColumn3 = document.querySelector(".col-video3");
      if (videoColumn3) {
        videoColumn3.childNodes.forEach(node => {
          node.remove();
        });
        videoComponent3.volume = 0;
        videoComponent3.width = screenWidth / 8;
        videoComponent3.height = screenHeight / 8;

        this.$nextTick(() => {
          videoColumn3.appendChild(videoComponent3);
        });
      }
      const videoColumn4 = document.querySelector(".col-video4");
      if (videoColumn4) {
        videoColumn4.childNodes.forEach(node => {
          node.remove();
        });
        videoComponent4.volume = 0;
        videoComponent4.width = screenWidth / 8;
        videoComponent4.height = screenHeight / 8;

        this.$nextTick(() => {
          videoColumn4.appendChild(videoComponent4);
        });
      }
    });
  }

  changeVideo(clickdedIndex: number) {
    this.activeVideoIndex = clickdedIndex;
    console.log(`activeVideoIndex : ${this.activeVideoIndex}`);

    let skipCount = 0;
    let mainVideoComponent: HTMLVideoElement;
    const subVideoComponent: Array<HTMLVideoElement> = [];
    [0, 1, 2, 3].forEach(subIndex => {
      if (subIndex == this.activeVideoIndex) {
        console.log(`${subIndex} : main`);
        mainVideoComponent = this.videoComponents[subIndex];
        if (this.hlsComponents[subIndex]) {
          this.hlsComponents[subIndex].autoLevelCapping = -1;
          this.hlsComponents[subIndex].nextLevel = -1;
        }
      } else {
        console.log(`${subIndex} : sub ${skipCount}`);
        subVideoComponent[skipCount] = this.videoComponents[subIndex];
        if (this.hlsComponents[subIndex]) {
          this.hlsComponents[subIndex].autoLevelCapping = 0;
          this.hlsComponents[subIndex].nextLoadLevel = 0;
          this.hlsComponents[subIndex].nextLevel = 0;
        }
        skipCount++;
      }
    });

    this.$nextTick(() => {
      this.setController(
        mainVideoComponent,
        subVideoComponent[0],
        subVideoComponent[1],
        subVideoComponent[2]
      );
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
  width: 800px+100px;
  height: 450px;
  background-color: black;
  display: flex;
}
.col-video-main {
  width: 800px;
  margin: 0;
  padding: 0;
  background-color: burlywood;
}
.col-video1 {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  float: right;
  background-color: bisque;
}
.col-video-sub {
  width: 100px;
  height: 450px;
  align-items: flex-end;
  background-color: pink;
  display: flex;
  flex-direction: column;
}
.col-video2 {
  background-color: lightblue;
}
.col-video3 {
  background-color: greenyellow;
}
.col-video4 {
  background-color: coral;
}
.playertools {
  background-color: white;
}
</style>
