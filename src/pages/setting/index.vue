<template>
  <view>

    <view class="head">
      <image class="logo" mode="scaleToFill" src="/static/image/logo.png" />
      <view class="name">公交快查</view>
    </view>

    <view class="listTitle">设置</view>
    <view class="list">
      <view class="item" @click="bindSettingTap">
        <view class="main">设置</view>
        <view class="sub">如果不能查询请点这里，允许使用地址位置</view>
      </view>
      <view class="item" @click="bindClearTap">
        <view class="main">清除缓存</view>
        <view class="sub">清空历史查询记录</view>
      </view>
    </view>

    <view class="listTitle">操作</view>
    <view class="list">
      <view class="item">
        <view class="main">分享</view>
        <view class="sub">赠人玫瑰，手有余香</view>
        <button class="button-share" open-type="share"></button>
      </view>
    </view>

    <!-- <view class="listTitle">新功能</view>
    <view class="list">
      <view class="item" @click="bindSoterTap">
        <view class="main">生物认证</view>
        <view class="sub">SoterAuthentication</view>
      </view>
    </view> -->

  </view>
</template>

<script>
import store from "@/store";

export default {
  data() {
    return {};
  },

  components: {},

  methods: {
    bindSettingTap() {
      wx.openSetting({
        success: res => {
          let authSetting = res.authSetting;
          if (authSetting["scope.userLocation"]) {
          } else {
            wx.showToast({
              title: "地理位置未开启，无法正常查询",
              icon: "none"
            });
          }
        }
      });
    },

    bindClearTap() {
      const success = res => {
        if (res.tapIndex == 0) {
          // wx.clearStorageSync();
          store.commit("route/clearHistorys");
        }
      };

      wx.showActionSheet({
        itemList: ["清空"],
        itemColor: "#FF3B30",
        success
      });
    }

    // bindSoterTap() {
    //   wx.checkIsSupportSoterAuthentication({
    //     success(res) {
    //       console.log(res);
    //       // res.supportMode = [] 不具备任何被SOTER支持的生物识别方式
    //       // res.supportMode = ['fingerPrint'] 只支持指纹识别
    //       // res.supportMode = ['fingerPrint', 'facial'] 支持指纹识别和人脸识别
    //     }
    //   });

    //   wx.startSoterAuthentication({
    //     requestAuthModes: ["facial"],
    //     challenge: "123456",
    //     authContent: "请用指纹解锁",
    //     success(res) {
    //       console.log(res);
    //     },
    //     fail(err) {
    //       console.log(err);
    //     }
    //   });
    // }
  },

  created() {},

  onShareAppMessage(res) {
    return {
      title: "快速查询公交地铁路线、附近站点信息，出行好帮手。",
      path: "/pages/route/main",
      imageUrl: "/static/image/logo.png"
    };
  }
};
</script>

<style lang="less">
@import "./index.less";
</style>
