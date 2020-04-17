<template>
  <v-content>
    <MultiViewMoviePlayer
      videopath="http://distribution.bbb3d.renderfarming.net/video/mp4/bbb_sunflower_1080p_30fps_normal.mp4"
    />
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
