<template>
  <view class="page">
    <view class="message-height">
      <view v-for="(it,i) in messageData" :key="i">
        <left-message v-if="it.type == -1" :data="it.message" time="14:00"></left-message>
        <right-message v-if="it.type == 1" :data="it.message" time="14:00"></right-message>
      </view>
    </view>
    <view class="footer">
      <sendMessage></sendMessage>
    </view>
  </view>
</template>

<script>
var messageDate = require("@/common/js/message.js");
import leftMessage from "../../components/left-message.vue";
import rightMessage from "../../components/right-message.vue";
import sendMessage from "../../components/send-message.vue";
export default {
  components: { leftMessage, rightMessage, sendMessage },
  data() {
    return {
      messageData: messageDate.messagelist,
      pageHeight: 0,
      windowHeight: 0
    };
  },
  onReady() {
    let _this = this;
    uni
      .createSelectorQuery()
      .selectAll(".page")
      .boundingClientRect(function(rects) {
        _this.pageHeight = rects[0].height;
        uni.pageScrollTo({
          scrollTop: rects[0].height - _this.windowHeight,
          duration: 0
        });
      })
      .exec();
  },
  onLoad() {
    let _this = this;
    uni.getSystemInfo({
      success: function(res) {
        _this.windowHeight = res.windowHeight;
      }
    });
    uni.onWindowResize(res => {
      uni.pageScrollTo({
        scrollTop: _this.pageHeight - res.size.windowHeight,
        duration: 300
      });
    });
  },
  methods: {}
};
</script>

<style lang="scss" scoped="true">
.page {
  box-sizing: border-box;
  width: 100%;
  background: #f6fbff;
  overflow: hidden;
  padding-bottom: 100upx;
}
.messagelist {
  .message-height {
    padding: 30upx 0 130upx 0;
  }
}
.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100upx;
}
</style>
