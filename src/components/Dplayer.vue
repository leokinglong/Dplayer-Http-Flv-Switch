<template>
  <div>
    <div id="liveInMiGu"></div>
  </div>
</template>

<script>
import DPlayer from "dplayer";
import Flv from "flv.js";
import Hls from "hls.js";
export default {
  name: "DPlayer",
  components: {},
  props: {},
  data() {
    return {
      dp: null
    };
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {
    let dp = this.initPlayer('http://172.28.22.90:8000/live/0_pvwplay278.flv')
    this.dp = dp
  },
  methods: {
    initPlayer(url){
      let _this = this
      // if (this.dp !== undefined && this.dp !== null && this.dp.video !== undefined && this.dp.video !== null){
      //   this.dp.video.src = url
      // }
      let options={
        container: document.getElementById("liveInMiGu"),
        autoplay: true,
        screenshot: true,
        hotkey: false,
        live: true,
        // enableStashBuffer: true,
        // volume: 0,
        video:{
          url: url,
          type: "customFlv",
          quality: [
            {
              name: "高清",
              url: "http://172.28.22.90:8000/live/0_pgmplay278.flv",
              type: "customFlv",
            },
            {
              name: "低清",
              // url: 'http://172.28.22.90:8000/live/0_pvwplay278.flv',
              url: 'http://172.28.22.90:8000/live/2_pgmplay277.flv',
              type: "customFlv",
            }
          ],
          defaultQuality: 0,
          customType: {
            customHls: (video, player) => {
              const hls = new Hls({
                p2pConfig: {
                  live: true, // 如果是直播设为true
                },
              });
              hls.loadSource(video.src);
              hls.attachMedia(video);
              player.events.on("destroy", () => {
                hls.destroy();
              });
            },
            customFlv: function (video, player) {
              const flvPlayer = Flv.createPlayer({
                type: "flv",
                url: url,
              });
              console.log(flvPlayer)
              // flvPlayer.currentTime = 1
              flvPlayer.attachMediaElement(video);
              flvPlayer.load();
              _this.flv = flvPlayer
              // flvPlayer.play();
              // flvPlayer.reload();
              player.events.on("destroy", () => {
                flvPlayer.unload();
                flvPlayer.detachMediaElement();
                flvPlayer.destroy();
              });
            },
          },
        }
      }
      // this.$nextTick(()=>{
      const dp = new DPlayer(options);
      const dom = document.getElementById("liveInMiGu")
      dom.getElementsByClassName('dplayer-quality-item')[0].ontouchend = (e) => {
        console.log('0000000')
        console.log(e)
        this.switchSrc('http://172.28.22.90:8000/live/0_pgmplay278.flv')
        dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].innerText = '高清'
        dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].innerHTML = '高清'
        dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].outerText = '高清'
        // dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].textContent = '高清'
        dom.getElementsByClassName('dplayer-quality')[0].style = 'color: #fff;  width: auto; line-height: 22px;font-size: 14px;'
        return false
      }
      dom.getElementsByClassName('dplayer-quality-item')[1].ontouchend = (e) => {
        console.log('1111111')
        console.log(e)
        // this.switchSrc('http://172.28.22.90:8000/live/0_pvwplay278.flv')
        this.switchSrc('http://172.28.22.90:8000/live/2_pgmplay277.flv')
        dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].innerText = '低清'
        dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].innerHTML = '低清'
        dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].outerText = '低清'
        // dom.getElementsByClassName('dplayer-icon dplayer-quality-icon')[0].textContent = '低清'
        dom.getElementsByClassName('dplayer-quality')[0].style = 'color: #fff;  width: auto; line-height: 22px;font-size: 14px;'
        return false
      }
      dom.getElementsByClassName('dplayer-mobile-play')[0].style = 'display:none'
      return dp
      // })
    },
    switchSrc(url){
     // this.dp.switchVideo({
     //   url: 'http://172.28.22.90:8000/live/0_pvwplay278.flv',
     //   type: "customFlv",
     //  })
      console.log(url)
      let playUrl = 'http://172.28.22.90:8000/live/0_pgmplay278.flv'
      if (url !== '' ) {
        playUrl = url
      }
      this.dp.destroy()
      let newPlayer = this.initPlayer(playUrl)
      const that = this
      newPlayer.on('canplay', function () {
        console.log('player canplay');
        // that.dp.destroy()
        that.dp = newPlayer
      });
    }
  },
};
</script>

<style scoped lang="scss">
</style>
