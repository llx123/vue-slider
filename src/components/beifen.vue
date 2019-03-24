<template>
  <div class="content">
    <div class="Box_con clearfix">
      <span class="btnl btn" id="btnl" @click="toLeft"></span>
      <span class="btnr btn" id="btnr" @click="toRight"></span>
      <div class="bigPic">
        <ul ref="tUl">
          <li class="cur" v-for="(i, index) in item.slice(0,3)" :key="index">
            <img :src="i" alt="">
          </li>
        </ul>
      </div>
      <div class="conbox" id="BoxUl">
        <ul ref="sUl">
          <li class="cur" :style="{border: index === 1 ? '2px solid red': 'none'}"
              v-for="(item, index) in item" :key="index" @click="doSwitch(index)"
            >
            <img :src="item" alt="">
          </li>              
        </ul>
      </div>    			    			
    </div>
  </div>
</template>

<script>
import img1 from '../assets/1.jpg'
import img2 from '../assets/2.jpg'
import img3 from '../assets/3.jpg'
import img4 from '../assets/4.jpg'
import img5 from '../assets/5.jpg'

export default {
  name: "HelloWorld",
  data() {
    return {
      nowIndex: 1,
      item: [img1,img2,img3,img4,img5],
      ulWidth: 0,
      tw: 0,
      htmlWidth: 0,
      canClick: true
    };
  },
  mounted() {
    this.setUlWidth()
    window.addEventListener('resize',()=>{
      this.setUlWidth()
    })
  },
  watch: {
    nowIndex(val, oldVal) {
      let len = this.item.length
      const uDom = this.$refs['sUl']
      const tDom = this.$refs['tUl']
      if((val > oldVal || (val === 0 && oldVal === len)) && !(val === len && oldVal === 0)) {
        uDom.style.transition = 'all linear .5s'
        tDom.style.transition = 'all linear .5s'
        uDom.style.transform = `translateX(-${ this.ulWidth + this.htmlWidth }px)`
        tDom.style.transform = `translateX(-${ this.tw*2 + this.htmlWidth }px)`
        setTimeout(()=>{
          uDom.style.transition = ''
          tDom.style.transition = ''
          let arr = []
          arr = this.item.slice(1, len)
          arr.push(this.item[0])
          this.item = arr
          uDom.style.transform = `translateX(0)`
          tDom.style.transform = `translateX(-${ this.tw + this.htmlWidth }px)`
        },500)
      } else {
        let arr = []
        arr = this.item.slice(0, len-1)
        arr.unshift(this.item[len-1])
        this.item = arr
        uDom.style.transition = ''
        tDom.style.transition = 'all linear .5s'
        uDom.style.transform = `translateX(-${ this.ulWidth + this.htmlWidth }px)`
        tDom.style.transform = `translateX(0)`
        setTimeout(()=>{          
          tDom.style.transition = ''
          uDom.style.transform = `translateX(0)`
          // uDom.style.transition = 'all linear .5s'
          // tDom.style.transform = `translateX(-${ this.tw + this.htmlWidth }px)`
        },50)        
      }
    }
  },
  methods: {
    doSwitch(index) {
      if(index == 2) {
        this.toRight()
      } else if(index == 0) {
        this.toLeft()
      }
      
    },
    setUlWidth() {
      const uDom = this.$refs['sUl']
      const tDom = this.$refs['tUl']
      let htmlWidth = document.documentElement.clientWidth || document.body.clientWidth;
      let len = this.item.length;
      if(!uDom && !tDom){
        return
      }
      let w = uDom.querySelector('li').offsetWidth
      let tw = tDom.querySelector('li').offsetWidth
      uDom.style.width = len * w + (htmlWidth / 32) * 0.625 * len + 'px'      
      tDom.style.width = len * tw + (htmlWidth / 32) * 0.625 * 3 + 'px'      
      uDom.style.transition = 'all linear .5s'
      tDom.style.transition = ''
      tDom.style.transform = `translateX(-${ tw + this.htmlWidth }px)`
      this.htmlWidth = (htmlWidth / 32) * .625
      this.ulWidth = w
      this.tw = tw
    },
    toRight() {
      this.doOnce()
      if(!this.canClick) return
      this.canClick = false
      let len = this.item.length
      if(this.nowIndex < len) {
        this.nowIndex = this.nowIndex + 1
      } else {
        this.nowIndex = 0
      }
    },
    toLeft() {
      this.doOnce()
      if(!this.canClick) return
      this.canClick = false
      let len = this.item.length
      if(this.nowIndex > 0) {
        this.nowIndex = this.nowIndex - 1
      } else {
        this.nowIndex = len
      }
    },
    doOnce() {            
      setTimeout(()=>{
        this.canClick = true
      },500)
    }
  },
  props: {
    msg: String
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='postcss' scoped>
* {padding: 0;margin: 0; box-sizing: border-box;}
body {background: #f3f3f3;}
.content {width: 718px;position: relative;margin: 0 auto;}
.Box_con {position: relative;}
.Box_con .btnl {position: absolute;}
.Box_con .btn {display: block;width: 41px;height: 41px;position: absolute;top: 280px;cursor: pointer;background: #ccc;}
.Box_con .btnl {left: -72px;}
.Box_con .btnr {right: -72px;}
.Box_con .btnl:hover {}
.Box_con .btnr:hover {}
.Box_con .conbox {position: relative;overflow: hidden;}
.Box_con .conbox ul {position: relative;list-style: none;}
.Box_con .conbox ul li {float: left;width: 225px;height: 200px;margin-left: 20px;overflow: hidden;}
.Box_con .conbox ul li:first-child {margin-left: 0;}
.Box_con .conbox ul li img {display: block;width: 225px;height: 200px;transition: all 0.5s;}
.Box_con .conbox ul li:hover img {transform: scale(1.1);}

.BoxSwitch {margin-top: 30px;text-align: center;}
.BoxSwitch span {display: inline-block;*display: inline;*zoom: 1;vertical-align: middle;width: 30px;height: 3px;background: #ccc;margin: 0 5px;cursor: pointer;}
.BoxSwitch span.cur {background: red;}

.bigPic {position: relative;overflow: hidden;height: 200px;}
.bigPic ul {position: relative;list-style: none;}
.bigPic ul li {float: left;width: 718px;height: 200px;margin-left: 20px;overflow: hidden;word-break: break-all;}
.bigPic ul li:first-child {margin-left: 0;}
.bigPic ul li img {display: block;width: 100%;height: auto;transition: all 0.5s;}

</style>
