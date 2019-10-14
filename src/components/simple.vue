/*
 * @Author: fangsp
 * @Date: 2019-10-13 16:11:28
 * @Last Modified by: mikey.zhaopeng
 * @Last Modified time: 2019-10-13 17:15:02
 */
<template>
  <div>
    <input type="file" @change="fileChange($event)">
    <!-- 截屏区域 -->
    <div id="myCanvas" class="fd-myCanvas">
      <!-- 头像 -->
      <img id="picture" :src="ylUrl" class="fd-picture" alt="">
      <!-- 小红旗 -->
      <img src="../assets/redQ.jpeg" alt="" class="fd-redQ" v-show="ylUrl" :style="styleObject">
    </div>
    <div>
      <!-- 最终合成的图片 -->
      <img :src="canvasUrl" alt="">
    </div>
  </div>
</template>

<script>
import html2canvas from 'html2canvas'
export default {
  name: 'simple',
  data () {
    return {
      canvasUrl: '',
      ylUrl: '',
      styleObject: {}
    }
  },
  methods: {
    toCanvas () {
      let _this = this
      let contentHtml = document.getElementById('myCanvas') // 要转化的div
      let width = contentHtml.offsetWidth
      let height = contentHtml.offsetHeight
      var opts = {
        width: width, // dom 原始宽度
        height: height // dom 原始高度
      }
      html2canvas(contentHtml, opts).then(function (canvas) {
        _this.canvasUrl = canvas.toDataURL()
      })
    },
    fileChange (e) {
      let _this = this
      let file = e.target.files
      const reader = new FileReader()
      reader.onload = function (ev) {
        // base64码
        _this.ylUrl = ev.target.result // 或e.target都是一样的
        setTimeout(() => {
          let picture = document.getElementById('myCanvas')
          let width = picture.offsetWidth / 4
          let height = picture.offsetHeight / 4
          // 设置小红旗的大小和位置，以便保持小红旗在头像的右下角
          _this.styleObject = {
            width: width + 'px',
            height: height + 'px'
          }
          _this.$nextTick(() => {
            _this.toCanvas()
          })
        })
      }
      // 发起异步读取文件请求，读取结果为data:url的字符串形式，
      reader.readAsDataURL(file[0])
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.fd-redQ {
  position: absolute;
  right: 0;
  bottom: 0;
}
.fd-myCanvas {
  position: fixed;
  top: 0;
  right: 0;
}
.fd-picture {
  max-width: 200px;
  max-height: 200px;
}
</style>
