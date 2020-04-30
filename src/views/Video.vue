<template>
  <v-content>
    <MultiViewMoviePlayer
      videopath1="ミクリンレンでハレ晴レユカイ1_x264_low.mp4"
      videopath2="ミクリンレンでハレ晴レユカイ2_x264_low.mp4"
      videopath3="ミクリンレンでハレ晴レユカイ3_x264_low.mp4"
      videopath4="ミクリンレンでハレ晴レユカイ4_x264_low.mp4"
    />
    <!-- MultiViewMoviePlayer
      videopath2="http://3qjcxj7d6rmwu6.data.mediastore.ap-northeast-1.amazonaws.com/livea/main.m3u8"
      videopath1="https://bitdash-a.akamaihd.net/content/MI201109210084_1/m3u8s/f08e80da-bf1d-4e3d-8899-f0f6155f6efa.m3u8"
      videopath3="https://bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8"
    /-->
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
