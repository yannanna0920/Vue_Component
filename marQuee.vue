<template>
  <div class="wrap">
    <!-- 表格操作的切换 -->
    <Icon
      type="ios-arrow-back"
      class="arrowLeft"
      @click="arrowL"
      v-if="this.pictureKey.length -3 > 0"
      :title="this.fisrtlist?`${$t('fisrtImage')}`:`${offsetCount+1}/${this.pictureKey.length}`"
    />
    <div class="wrapImage">
      <div class="imgList" ref="imgList">
        <img
          :src="`${getUrl(value)}?width=${width}&height=${height}`"
          v-for="(value,index) in pictureKey"
          :key="index"
          :width="width"
          :height="height"
          @click="bigImg(currentImage,index)"
          :title="`${$t('howmany').replace('%s',index+1)}`"
        >
      </div>
    </div>
    <Icon
      type="ios-arrow-forward"
      class="arrowRight"
      @click="arrowR"
      v-if="this.pictureKey.length -3 > 0"
      :title="this.lastlist?`${$t('lastImage')}`:`${offsetCount+1}/${this.pictureKey.length}`"
    />
    <!-- 点击后的大图 -->
    <div class="imgMask" v-if="showBigImg">
      <Icon
        type="md-close"
        class="close"
        @click.stop="showBigImg=!showBigImg"
        :title="$t('close')"
      />
      <Icon
        class="prev"
        type="ios-arrow-dropleft"
        @click.stop="prev"
        :title="this.fisrtlist?`${$t('fisrtImage')}`:`${num+1}/${this.pictureKey.length}`"
      />
      <div class="showImg">
        <img class="bigImg" :src="getUrl(pictureKey[num])">
      </div>
      <Icon
        type="ios-arrow-dropright"
        class="next"
        @click.stop="next"
        :title="this.lastlist?`${$t('lastImage')}`:`${num+1}/${this.pictureKey.length}`"
      />
      <span
        class="rightTooltip"
      >{{`${$t('howmany').replace('%s',num+1)} / ${$t('EditionWorkflowStatus:total')}${this.pictureKey.length}`}}</span>
    </div>
  </div>
</template>
<script>
import util from "@/libs/util";
import imageView from "@/components/imageView";
export default {
  name: "Marquee",
  props: {
    pictureKey: Array,//图片的地址
    width: Number,
    height: Number
  },
  data() {
    return {
      showBigImg: false,//是否显示大图
      num: 0,//记录是第几张 大图
      currentImage: "",//当前图片
      offsetCount: 0,//记录是第几张 表格小图
      fisrtlist: true,//是否为第一张 初始状态是真
      lastlist: false//是否为最后一张 初始状态是假
    };
  },
  methods: {
    bigImg(currentImage, index) {
      this.currentImage = currentImage;
      this.showBigImg = true;
      this.num = index;
      if (index === 0) {
        this.fisrtlist = true;
      } else {
        this.fisrtlist = false;
      }
      if (index + 1 < this.pictureKey.length) {
        this.lastlist = false;
      } else {
        this.lastlist = true;
      }
    },
    prev() {
      //上一张
      if (this.num == 0) {
        this.lastlist = true;
        this.fisrtlist = false;
        this.num = this.pictureKey.length;
      } else {
        this.fisrtlist = false;
        this.lastlist = false;
      }
      this.num--;
    },
    next() {
      //下一张
      if (this.num == this.pictureKey.length - 1) {
        this.lastlist = false;
        this.fisrtlist = true;
        this.num = -1;
      } else {
        this.lastlist = false;
        this.fisrtlist = false;
      }
      this.num++;
    },
    getUrl(pictkey) {//图片地址拼接
      return `${process.env.VUE_APP_FILE_URL}/${pictkey}`;
    },
    arrowL() {
      //列表的左箭头
      if (this.offsetCount > 0) {
        this.lastlist = false;
        this.offsetCount--;
        this.$refs.imgList.style.left = "-" + 44 * this.offsetCount + "px";
      } else {
        this.fisrtlist = true;
        return false;
      }
    },
    arrowR() {
      //列表的右箭头
      if (this.offsetCount < this.pictureKey.length - 3) {
        this.fisrtlist = false;
        this.offsetCount++;
        this.$refs.imgList.style.left = "-" + 44 * this.offsetCount + "px";
      } else {
        this.lastlist = true;
        return false;
      }
    }
  }
};
</script>
<style lang="less" scoped>
.imgList {
  position: relative;
}
.imgList img {
  margin: 0 2px;
  user-select: none;
}
.imgList img:nth-child(odd) {
  border: 1px solid #bbbaba;
}
.imgList img:nth-child(even) {
  border: 1px solid black;
}
.imgMask {
  position: absolute;
  width: 100%;
  top: 0px;
  left: 0;
  z-index: 100;
  background: rgba(0, 0, 0, 0.6);
  user-select: none;
}
.showImg {
  width: 680px;
  position: relative;
  border: 5px solid #fff;
  border-radius: 3px;
  margin: 27px auto;
  background: #fff;
}
.bigImg {
  width: 100%;
  height: 100%;
}
.prev {
  position: absolute;
  top: 46%;
  font-size: 57px;
  color: #c8c9ca;
  z-index: 2;
  width: 62px;
}
.prev:hover {
  color: #e0e0e0;
  border-radius: 50%;
}
.next {
  position: absolute;
  top: 46%;
  right: -2px;
  font-size: 57px;
  color: #c8c9ca;
  z-index: 2;
  width: 62px;
}
.next:hover {
  color: #e0e0e0;
  border-radius: 50%;
}
.arrowLeft {
  font-size: 20px;
  color: black;
  z-index: 1;
  line-height: 42px;
}
.arrowRight {
  font-size: 20px;
  color: black;
  font-size: 20px;
  color: black;
  z-index: 1;
  position: absolute;
  right: 0;
  z-index: 1;
  line-height: 42px;
}
.wrapImage {
  width: 146px;
  overflow: hidden;
  white-space: nowrap;
}
.wrap {
  display: flex;
  height: 40px;
}
.close {
  font-size: 50px;
  color: #e9e9e9;
  float: right;
}
.close:hover {
  background: #b11f1f87;
}
.leftTooltip {
  position: absolute;
  bottom: 10px;
  left: 10px;
  padding: 5px 10px;
  color: #e9e9e9;
  font-size: 14px;
  font-weight: bold;
}
.rightTooltip {
  position: absolute;
  bottom: 1px;
  right: 10px;
  padding: 5px 10px;
  color: #e9e9e9;
  font-size: 14px;
  font-weight: bold;
}
</style>


