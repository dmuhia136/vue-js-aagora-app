<template>
    <div>
        UID:{{ user.uid }}
        <div ref="video" class="video" >
        </div>
            <button @click="pause">Stop Video</button>
            <button @click="start">Resume Video </button>
            <button @click="mute">Mute</button>
            <button @click="resumeAudio">Unmute</button>
    </div>
</template>
<script setup>
import { onMounted, ref,onCreated,onBeforeMount } from 'vue';
const props = defineProps(['user'])
const video = ref(null)
const pauseVideo = ref(null)
onBeforeMount(async () => {
    await props.user.videoTracks.play(video.value)
    await props.user.audioTracks.play(video.value)
})
const pause=async()=>{
    await props.user.videoTracks.stop(video.value)
}
const start=async()=>{
    await props.user.videoTracks.play(video.value)
}
const mute=async()=>{
await props.user.audioTracks.stop(video.value)
}
const resumeAudio=async()=>{
    await props.user.audioTracks.play(video.value)
}
</script>


<style scoped>
.video {
    width: 400px;
    height: 400px;
}
</style>