<template>
  <v-content>
    <MultiViewMoviePlayer videopath1="4k.mp4" videopath2="2k.mp4" />
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
