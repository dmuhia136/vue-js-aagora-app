<template>
    <div>
        <div v-for="(user, i) in users" class="flex ">
            <VideoPlay :user="user" :uid="agora_uid" :key="i"/>
            
        </div>
    </div>
</template>
<script>
import AgoraRtc from 'agora-rtc-sdk-ng';
const APP_ID = 'ddc52c0fe419478588c8f97c4fa4f45b';
const CHANNEL = 'chat';
const TOKEN = '007eJxTYJgvxn2k33XjccXLN2zrr1zjuvtoZtV6/Q9v5p3W3TtfgC9QgSElJdnUKNkgLdXE0NLE3MLUwiLZIs3SPNkkLdEkzcQ0KeDg7uSGQEaGtBgbRkYGCATxWRiSMxJLGBgAx7kgvg==';
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
            this.agora_uid=uid;
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