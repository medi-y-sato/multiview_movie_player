<template>
  <v-content>
    <MultiViewMoviePlayer videopath1="video1.mp4" videopath2="video2.mp4" />
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
