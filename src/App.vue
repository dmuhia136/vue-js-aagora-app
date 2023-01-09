<template>
  <div>Vuejs Agora App
    <div v-if="userJoined">
      <VideoVue />
    </div>
    <div v-else>
      <button @click="joinRoom" className="p-2 font-bold">Join Channel</button>
    </div>
  </div>
</template>
<script >
import VideoVue from './components/Video.vue';

export default {
  components: {
    VideoVue
  },
  data() {
    return {
      userJoined: false,
    }
  },
  methods: {
    joinRoom() {
      this.userJoined = true
    }
  }
}
</script>
<style scoped>

</style>
