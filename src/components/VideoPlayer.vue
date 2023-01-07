<template>
    <!-- UID:{{user}} -->
    <p>nnjk</p>
    <div ref="video" class="video">

    </div>
</template>
<script>
export default{
    // props:{
    //     user:Array
    // },
    // created(){
    //     this.$props.user.videoTracks.play(this.$refs.video)
    //     console.log("my client",this.$props.user);
    // }
}
</script>

<style scoped>
.video{
    width: 200px;
    height: 200px;
    background-color: red;
}
</style>