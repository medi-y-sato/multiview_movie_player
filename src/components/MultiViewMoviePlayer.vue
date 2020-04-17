<template>
  <v-container class="fill-height fill-width" fluid>
    <v-row align="streach" justify="space-around">
      <v-col class="text-center" justify="space-around">
        <video controls class="el-video1" width="100%" height="100%">
          <source :src="videopath" />
        </video>
        <div>
          <button @click="clickPlaybutton(1)">
            <v-icon>{{btnstate}}</v-icon>
          </button>
        </div>
      </v-col>
      <v-col class="text-center" justify="space-around">
        <video controls class="el-video2" width="100%" height="100%">
          <source :src="videopath" />
        </video>
        <div>
          <button @click="clickPlaybutton(2)">
            <v-icon>{{btnstate}}</v-icon>
          </button>
        </div>
      </v-col>
    </v-row>
    <v-row align="streach" justify="space-around">
      <v-col class="text-center" justify="space-around">
        <video controls class="el-video3" width="100%" height="100%">
          <source :src="videopath" />
        </video>
        <div>
          <button @click="clickPlaybutton(3)">
            <v-icon>{{btnstate}}</v-icon>
          </button>
        </div>
      </v-col>
      <v-col class="text-center" justify="space-around">
        <video controls class="el-video4" width="100%" height="100%">
          <source :src="videopath" />
        </video>
        <div>
          <button @click="clickPlaybutton(4)">
            <v-icon>{{btnstate}}</v-icon>
          </button>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class MultiViewMoviePlayer extends Vue {
  play = false;
  btnstate = "mdi-play";

  @Prop() private videopath!: string;

  clickPlaybutton(playernumber: number) {
    const videoComponent: Element = document.querySelector(
      ".el-video" + playernumber
    )!;
    if (videoComponent instanceof HTMLMediaElement) {
      if (this.play == true) {
        videoComponent.pause();
        this.btnstate = "mdi-play";
        this.play = false;
      } else {
        videoComponent.play();
        this.btnstate = "mdi-pause";
        this.play = true;
      }
    } else {
      throw new Error(".el-video is not video");
    }
  }
}
</script>

<style scoped></style>
