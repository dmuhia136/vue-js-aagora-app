<template>
    <div>Video App
        <div v-for="(user, i) in users" >
            <VideoPlay :user="user" :key="i"/>
            
        </div>
    </div>
</template>
<script>
import AgoraRtc from 'agora-rtc-sdk-ng';
const APP_ID = 'ddc52c0fe419478588c8f97c4fa4f45b';
const CHANNEL = 'chat';
const TOKEN = '007eJxTYNj72W87v/PLKS+Osh7Zs+iT+cmabT+Mf61ufvx+xtdkfc9oBYaUlGRTo2SDtFQTQ0sTcwtTC4tkizRL82STtESTNBPTJGfznckNgYwM0ztPMzEyQCCIz8KQnJFYwsAAAJV+I34=';
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
}
</script>

<script setup>
import VideoPlay from './Videoplay.vue'
</script>

<style scoped>
.video {
    width: 200px;
    height: 200px;
}
</style>