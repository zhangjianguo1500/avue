<template>
  <div :class="b()"
       v-if="isShow">
    <div :class="b('mask')"></div>
    <span class="el-image-viewer__btn el-image-viewer__close"
          @click="close">
      <i class="el-icon-circle-close"></i>
    </span>
    <span class="el-image-viewer__btn el-image-viewer__prev"
          @click="$refs.carousel.prev()"
          v-if="isRrrow">
      <i class="el-icon-arrow-left"></i>
    </span>
    <span class="el-image-viewer__btn el-image-viewer__next"
          @click="$refs.carousel.next()"
          v-if="isRrrow">
      <i class="el-icon-arrow-right"></i>
    </span>
    <div :class="b('box')"
         ref="box"
         :style="styleBoxName">
      <el-carousel ref="carousel"
                   :initial-index="index"
                   :interval="0"
                   arrow="never"
                   @change="handleChange"
                   indicator-position="none"
                   :height="height">
        <el-carousel-item v-for="(item,index) in datas"
                          :key="index">
          <img :src="item.url"
               :style="styleName"
               @mousedown="move"
               ondragstart="return false"></img>
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="el-image-viewer__btn el-image-viewer__actions">
      <div class="el-image-viewer__actions__inner">
        <i class="el-icon-zoom-out"
           @click="scale=scale-0.2"></i>
        <i class="el-icon-zoom-in"
           @click="scale=scale+0.2"></i>
        <!-- <i class="el-image-viewer__actions__divider"></i> -->
        <!-- <i class="el-icon-full-screen"></i> -->
        <!-- <i class="el-image-viewer__actions__divider"></i> -->
        <i class="el-icon-refresh-left"
           @click="rotate=rotate-90"></i>
        <i class="el-icon-refresh-right"
           @click="rotate=rotate+90"></i>
      </div>
    </div>
  </div>
</template>
<script>
import create from "core/create";
export default create({
  name: "image-preview",
  data () {
    return {
      left: 0,
      top: 0,
      scale: 1,
      datas: [],
      rotate: 0,
      isShow: false,
      index: 0,
      onClose: null,
    };
  },
  computed: {
    styleBoxName () {
      return {
        marginLeft: this.setPx(this.left),
        marginTop: this.setPx(this.top)
      }
    },
    styleName () {
      return {
        transform: `scale(${this.scale}) rotate(${this.rotate}deg)`,
        maxWidth: '100%',
        maxHeight: '100%',
      }
    },
    isRrrow () {
      return this.imgLen != 1
    },
    imgLen () {
      return this.imgList.length
    },
    imgList () {
      return this.datas.map(ele => ele.url)
    }
  },
  methods: {
    handleChange () {
      this.scale = 1;
      this.rotate = 0;
    },
    move (e) {       //获取目标元素s
      //算出鼠标相对元素的位置
      let disX = e.clientX;
      let disY = e.clientY;
      document.onmousemove = (e) => {       //鼠标按下并移动的事件
        //用鼠标的位置减去鼠标相对元素的位置，得到元素的位置
        let left = e.clientX - disX;
        let top = e.clientY - disY;
        disX = e.clientX;
        disY = e.clientY;
        //移动当前元素
        this.left = this.left + left
        this.top = this.top + top
      };
      document.onmouseup = (e) => {
        document.onmousemove = null;
        document.onmouseup = null;
      };
    },
    close () {
      this.isShow = false
      if (typeof this.onClose === "function") {
        this.onClose(this);
      }
    }
  }
});
</script>