<template>
  <div class="extract-word">
    <h2>上传图片，提取图片文字内容</h2>
    <div class="upload-box">
      <div class="upload-showed">
        <img id="img-word" :src="upImg" alt="">
        <!--<i :style="{background: upImg, 'display': 'inline-block', width: '100px', height: '100px' }"></i>-->
      </div>
      <div class="upload-icon">
        <label for="up-img" class="up-img" :style="{ background: 'url(' + './static/image/image.png' + ') no-repeat 100% 100%'}">
          <input type="file" id="up-img" name="up-img" :value="upValue" @change="handleChangeFunc($event,upValue)">
        </label>
        <img :src="'./static/image/photo.png'" alt="">
        <button @click="confirmCommitFunc" class="up-btn" style="">确认提交</button>
        <!--<img :src="'./static/image/image.png'" alt="">-->
      </div>
    </div>
    <div class="text-box">
      <!--<textarea name="" id="" cols="30" rows="10"></textarea>-->
      {{upText}}
    </div>
  </div>
</template>

<script type="text/javascript">
  // import Tesseract from 'tesseract.js'
export default {
  name: 'ExtractImageWord',
  data () {
    return {
      upValue: null,
      upImg: '',
      upText: ''
    }
  },
  methods: {
    handleChangeFunc (e, val) {
      var Tesseract = require('tesseract.js')
      const that = this
      console.log(e)
      console.log('-----------')
      console.log(val)
      const file = new FileReader()
      file.readAsDataURL(e.target.files[0])
      file.onload = function (result) {
        console.log('55555')
        console.log(result)
        // that.upImg = `url(${this.result}) no-repeat 100% 100%`
        that.upImg = this.result
        console.log(that.upImg)
        // Tesseract.recognize(document.getElementById('img-word')).then(function (res) {
        //   console.log('=============')
        //   console.log(res)
        // })
        Tesseract.recognize(this.result)
          .progress(message => console.log(message))
          .catch(err => console.error(err))
          .then(result => {
            console.log(result)
            that.upText = result
          })
          .finally(resultOrError => console.log(resultOrError))
      }
    },
    confirmCommitFunc () {
      console.log(this.upValue)
    }
  }
}
</script>

<style scoped>
  .extract-word {
    width: 100%;
    height: 100%;
  }
  .extract-word h2{
    margin: 20px auto;
    text-align: center;
  }
  .upload-box{
    width: 80%;
    min-height: 200px;
    border: 1px solid cadetblue;
    margin: 50px auto;
    -webkit-border-radius: 8px;
    -moz-border-radius: 8px;
    border-radius: 8px;
  }
  .text-box{
    width: 80%;
    min-height: 200px;
    border: 1px solid deepskyblue;
    margin: 0px auto;
    cursor: text;
    -webkit-border-radius: 8px;
    -moz-border-radius: 8px;
    border-radius: 8px;
  }
  .upload-showed{
    width: 100%;
    min-height: 150px;
  }
  .upload-icon{
    width: 100%;
    height: 50px;
    line-height: 50px;
    text-align: center;
    position: relative;
  }
  .upload-icon img{
    margin: 0 20px;
    cursor: pointer;
  }
  #up-img{
    border: none;
    width: 50px;
    height: 50px;
    color: transparent;
    outline: none;
    overflow: hidden;
    opacity: 0;
    cursor: pointer;
    /*position: absolute;*/
    /*top: 0px;*/
    /*background-image: url('./static/image/image.png');*/
  }
  .up-img{
    display: inline-block;
    width: 50px;
    height: 50px;
    /*position: relative;*/
  }
  .up-btn{
    height: 28px;
    width: 100px;
    color: #fff;
    line-height: 28px;
    background-color: cadetblue;
    border: none;
    outline: none;
    cursor: pointer;
    position: absolute;
    top: 12px;
  }
  #img-word{
    max-width: 500px;
    max-height: 500px;
  }
</style>
