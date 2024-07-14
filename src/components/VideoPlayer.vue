<template>
  <div class="video-player">
    <video
      ref="videoPlayer"
      src="../assets/The Breathtaking Beauty of Nature - HD.mp4"
      @click="togglePlay()"
      @ended="restartVideo()"
      @timeupdate="updateTime()"
      @loadedmetadata="onLoadedMetadata()"
    ></video>
    <div class="controls">
      <button @click="togglePlay()">
        {{ isPlaying ? "||" : "▶" }}
      </button>
      <div class="progress-bar" @click="seek">
        <div class="progress" :style="{ width: progress + '%' }"></div>
      </div>
      <div class="time">{{ formattedTime }} / {{ formattedDuration }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isPlaying: false,
      currentTime: 0,
      videoDuration: 0,
    };
  },
  methods: {
    togglePlay() {
      if (this.isPlaying) {
        this.$refs.videoPlayer.pause();
      } else {
        this.$refs.videoPlayer.play();
      }
      this.isPlaying = !this.isPlaying;
    },
    restartVideo() {
      const video = this.$refs.videoPlayer;
      video.currentTime = 0;
      video.play();
    },
    updateTime() {
      if (this.$refs.videoPlayer) {
        this.currentTime = this.$refs.videoPlayer.currentTime;
      }
    },
    onLoadedMetadata() {
      this.videoDuration = this.$refs.videoPlayer.duration;
    },
    seek(event) {
      const progressBar = event.currentTarget;
      const clickPosition =
        event.clientX - progressBar.getBoundingClientRect().left;
      const percentClicked = clickPosition / progressBar.offsetWidth;
      this.$refs.videoPlayer.currentTime = percentClicked * this.videoDuration;
    },
    formatTime(time) {
      const minutes = Math.floor(time / 60);
      const seconds = Math.floor(time % 60);
      return `${minutes.toString().padStart(2, "0")}:${seconds
        .toString()
        .padStart(2, "0")}`;
    },
  },
  computed: {
    formattedTime() {
      return this.formatTime(this.currentTime);
    },
    formattedDuration() {
      return this.formatTime(this.videoDuration);
    },
    progress() {
      return (this.currentTime / this.videoDuration) * 100 || 0;
    },
  },
};
</script>

<style scoped>
.video-player {
  width: 600px;
  margin: 0 auto;
  padding: 3px;
  border: 5px solid rgb(219, 218, 218);
  border-radius: 7px;
}
video {
  width: 100%;
  border-radius: 5px;
}
.controls {
  display: flex;
  margin-top: 10px;
  text-align: left;
  background-color: #a4afb8;
  align-items: center;
  justify-content: space-between;
  margin: 0;
  height: 60px;
  border-radius: 7px;
}
button {
  margin-right: 20px;
  margin-left: 20px;
  width: 40px;
  height: 40px;
  padding: 10px;
  font-size: 16px;
  background-color: #374856;
  border-radius: 50%;
  color: aliceblue;
  outline: none;
  border: none;
}
.progress-bar {
  flex-grow: 1;
  height: 5px;
  background-color: #ddd;
  cursor: pointer;
  margin: 0 10px;
}
.progress {
  height: 100%;
  background-color: #374856;
}
.time {
  margin-right: 20px;
  font-size: 14px;
}
</style>
