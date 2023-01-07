<template>
    <div>
        <div ref="video" class="video" >
        </div>
        <button @click="pause">Pause</button>
        <button @click="start">Start</button>
    </div>
</template>
<script setup>
import { onMounted, ref } from 'vue';
const props = defineProps(['user'])
const video = ref(null)
const pauseVideo = ref(null)
onMounted(async () => {
    await props.user.videoTracks.play(video.value)
    await props.user.audioTracks.play(video.value)

})
const pause=async()=>{
    await props.user.videoTracks.stop(video.value)
    await props.user.audioTracks.stop(video.value)
}
const start=async()=>{
    await props.user.videoTracks.play(video.value)
    await props.user.audioTracks.play(video.value)
}
</script>


<style scoped>
.video {
    width: 400px;
    height: 400px;
}
</style>