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
    <img id="video-cover" src="../assets/GF-3.jpg" alt="Cover Image" @click="handleClick()" v-show="show" />
    <div id="tips" v-show="!show">横屏体验感更佳</div>
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
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data() {
    return {
      show: true,
      playUrl: '',
    }
  },
  mounted() {
    console.log(window.location)
    console.log(videoList)
    console.log(this.getQueryString(window.location.href, 'link'))
    const link = this.getQueryString(window.location.href, 'link')
    if (link) {
      this.playUrl = videoList.find(obj=>obj.link==link).playUrl || ''
    }
    console.log(this.playUrl)
    var vr = new VR({ id: this.$refs.player })
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
    vr.init(function () {})

    var options = { muted: false, autoplay: false }
    vr.play(
      this.playUrl,
      vr.resType.sliceVideo,
      options
    )
    vr.video.setAttribute('loop', 'loop')
    vr.video.crossOrigin = 'Anonymous'
    // vr.toolBar.vrBtn.style.display = 'none'
    console.log(vr.toolBar)

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
    // vr.playPanorama(require('../assets/puydesancy.jpg'))
  },
  methods: {
    handleClick() {
      this.show = false
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
  bottom: 30px;
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
