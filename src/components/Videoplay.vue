<template>
    <div>

        <div ref="video" :class="user.uid === uid ? 'w-screen h-screen ml-2 mr-2' : 'video relative bottom-0 right-0'">
        </div>
        <div class="flex gap-x-2 justify-center pt-2">
            <button @click="pause" class="text-red-400 p-2 buttonStyle">Stop Video</button>
            <button @click="start" class="buttonStyle">Resume Video </button>
            <button @click="mute" class="buttonStyle">Mute</button>
            <button @click="resumeAudio" class="buttonStyle">Unmute</button>
        </div>
    </div>
</template>
<script setup>
import { onMounted, ref, onBeforeMount } from 'vue';
const props = defineProps(['user', 'uid'])
const video = ref(null)
const pauseVideo = ref(null)
onMounted(async () => {
    await props.user.videoTracks.play(video.value)
    await props.user.audioTracks.play(video.value)
})
const pause = async () => {
    await props.user.videoTracks.stop(video.value)
}
const start = async () => {
    await props.user.videoTracks.play(video.value)
}
const mute = async () => {
    await props.user.audioTracks.stop(video.value)
}
const resumeAudio = async () => {
    await props.user.audioTracks.play(video.value)
}
</script>


<style scoped>
.video {
    width: 400px;
    height: 400px;
}
</style>