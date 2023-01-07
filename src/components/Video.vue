<template>
    <div>Video App
        <div v-for="(user, i) in users" >
            <!-- <VideoPlayerVue  /> -->
            <VideoPlay :user="user" :key="i"/>
            <!-- <div class="video" @change="videoPlayer(user)" :key="i" ref="video"> -->
            <!-- {{ user.videoTracks(user) }} -->

            <!-- </div> -->
        </div>
    </div>
</template>
<script>
import AgoraRtc from 'agora-rtc-sdk-ng';
import VideoPlayer from './VideoPlayer.vue';
const APP_ID = 'ddc52c0fe419478588c8f97c4fa4f45b';
const CHANNEL = 'chat';
const TOKEN = '007eJxTYNg73/GaRHLhysilL7/xyETaHv6Z4nUlV3b1wmqu7s1Nv6coMKSkJJsaJRukpZoYWpqYW5haWCRbpFmaJ5ukJZqkmZgmTXi0PbkhkJHBss+ChZEBAkF8FobkjMQSBgYAqlUgsQ==';
const client = AgoraRtc.createClient({ mode: 'rtc', codec: 'vp8' });

export default {

    data() {
        return {
            agora_uid: null,
            users: []
        };
    },
    mounted() {
        client.on("user-published", this.handleUserJoined);
        client.on("user-left", this.handleUserLeft);
        client.join(APP_ID, CHANNEL, TOKEN, null).then((uid) => Promise.all([AgoraRtc.createMicrophoneAndCameraTracks(), uid])).then(([tracks, uid]) => {
            console.log("agora ui", uid);
            const [audioTracks, videoTracks] = tracks;
            this.users = [...this.users, {
                uid,
                videoTracks,
                audioTracks
            }];
            client.publish(tracks);
        });

    },
    methods: {
        async handleUserJoined(user,mediaType) {
            await client.subscribe(user,mediaType)
            if(mediaType==='video'){
                this.users=[...this.users,this.users]
            }
        },
        async handleUserLeft(user) {
            this.user=this.user.filter((u)=>u.uid !==user.uid)
        },
        
        async loadVideo(user) {
            return user.videoTracks.play(this.$refs.video)
        }
    },
    components: { VideoPlayer }
}
</script>

<script setup>
import VideoPlayerVue from './VideoPlayer.vue';
import VideoPlay from './Videoplay.vue'
</script>

<style scoped>
.video {
    width: 200px;
    height: 200px;
}
</style>