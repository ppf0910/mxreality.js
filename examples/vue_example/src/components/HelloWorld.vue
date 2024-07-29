<!--
 * @Author: Pangpf
 * @Date: 2024-07-25 10:49:51
 * @LastEditors: Pangpf
 * @LastEditTime: 2024-07-25 17:15:28
 * @Description: 
-->
<template>
  <div class="hello">
    <div ref="player"></div>
    <div id="clock"></div>
    <img id="video-cover" src="../assets/GF-3.jpg" alt="Cover Image" @click="handleClick()" v-show="show" />
    <div id="tips" v-show="!show">横屏也可观看</div>
    <img id="logo-image" src="../assets/logo4.png" alt="Logo Image" v-show="!show" />
  </div>
</template>

<script>
// import * as Hls from 'mxreality.js/build/hls'
// import * as THREE from 'mxreality.js/build/three'
// import { VR, AVR } from 'mxreality.js/build/mxreality'

// window.Hls = Hls
// window.THREE = THREE
// window.VR = VR
// window.AVR = AVR

/* eslint-disable */
var vr = null
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data() {
    return {
      show: true,
      playUrl: '',
      timer: undefined,
    }
  },
  mounted() {
    const link = this.getQueryString(window.location.href, 'link')
    if (link) {
      this.playUrl = videoList.find((obj) => obj.link == link).playUrl || ''
    }
    console.log(this.playUrl)
  },
  methods: {
    handleClick() {
      this.show = false
      setInterval(this.updateClock, 1000)
      this.initVideo()
    },
    initVideo() {
      vr = new VR({ id: this.$refs.player })
      // vr.init(function () {})
      //renderer = new THREE.WebGLRenderer();
      if (
        navigator.userAgent.match(/iphone os 14_0/i) ||
        navigator.userAgent.match(/iphone os 14_1/i) ||
        navigator.userAgent.match(/iphone os 14_3/i)
      ) {
        AVR.__fixHlsRender = true
      }
      // var vr = new VR({ id: 'example' })

      //vr.playText="<img src='img/play90.png' width='40' height='40'/>";
      vr.vrbox.radius = 600
      if (AVR.isCrossScreen()) {
        // 调整vr视窗偏移量
        vr.effect.separation = 2
      }
      vr.loadProgressManager.onLoad = function () {
        // 视频静音
      }
      //AVR.useGyroscope=false;
      vr.init(function () {
      })

      var options = { muted: false, autoplay: true, preload: true, }
      vr.play(this.playUrl, vr.resType.sliceVideo, options)
      vr.hlsConfig = {
        autoStartLoad: true,
    };
      console.log(vr.video)
      vr.video.setAttribute('loop', 'loop')
      vr.video.crossOrigin = 'Anonymous'
      vr.controls.gyroUnfreeze()
      // vr.video.pause()

      // vr.toolBar.vrBtn.style.display = 'none'
      console.log(vr.toolBar.btn)
      vr.toolBar.btn.click()

      // vr.video.pause()
      vr.video.onended = function () {}
      vr.loadProgressManager.onLoad = function () {
        console.log('loaded.........')
      }
      vr.loadProgressManager.onProgress = function () {
        console.log('onProgress')
      }
      vr.loadProgressManager.onError = function () {
        console.log('onError')
      }
    },
    getQueryString(url, paraName) {
      const arrObj = url.split('?')
      if (arrObj.length > 1) {
        const arrPara = arrObj[1].split('&')
        let arr
        for (let i = 0; i < arrPara.length; i++) {
          arr = arrPara[i].split('=')
          // eslint-disable-next-line eqeqeq
          if (arr != null && arr[0] == paraName) {
            return arr[1]
          }
        }
        return ''
      } else {
        return ''
      }
    },
    updateClock() {
      const now = new Date()
      const year = now.getFullYear()
      const month = String(now.getMonth() + 1).padStart(2, '0')
      const day = String(now.getDate()).padStart(2, '0')
      const hours = String(now.getHours()).padStart(2, '0')
      const minutes = String(now.getMinutes()).padStart(2, '0')
      const seconds = String(now.getSeconds()).padStart(2, '0')
      const weekDays = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']
      const dayString = weekDays[now.getDay()]

      const timeString = `${year}-${month}-${day} ${hours}:${minutes}:${seconds} ${dayString}`
      document.getElementById('clock').textContent = timeString
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.hello {
  /* position: relative;
  height: 100%;
  width: 100%; */
}

#clock {
  position: absolute;
  top: 10px;
  right: 10px;
  z-index: 99;
  width: 100%;
  height: 30px;
  font-size: 16px;
  color: #fff;
  text-align: right;
}

#video-cover {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  cursor: pointer;
}
#tips {
  position: absolute;
  bottom: 35px;
  left: 10px;
  color: white;
  font-size: 14px;
  font-family: Arial, sans-serif;
  z-index: 10;
  pointer-events: none;
}
#logo-image {
  position: absolute;
  width: 380px;
  height: 100px;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  opacity: 0.2;
  pointer-events: none;
}
</style>
