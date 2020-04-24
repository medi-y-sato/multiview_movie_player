<template>
  <v-content>
    <MultiViewMoviePlayer
      videopath1="https://bitdash-a.akamaihd.net/content/MI201109210084_1/m3u8s/f08e80da-bf1d-4e3d-8899-f0f6155f6efa.m3u8"
      videopath2="https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8"
    />
    <p>
      video from
      <a href="https://www.pexels.com/videos/">Pexels</a>
    </p>
  </v-content>
</template>

<script>
// @ is an alias to /src
import MultiViewMoviePlayer from "@/components/MultiViewMoviePlayer.vue";

export default {
  name: "Video",
  components: {
    MultiViewMoviePlayer
  }
};
</script>
