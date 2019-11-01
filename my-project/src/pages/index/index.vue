<template>

  <div class="page">
    <!-- 音乐播放 -->
    <div class="music" style="width:50px;height:50px;" v-if="isshow">
      <audio :src="musicUrl" loop="loop" id="myAudio" style="display: none;"></audio>
      <img class="music_img" @click="handleMusicPlay()"
        :src="isMusicPlay?'/static/images/music-stop.png':'/static/images/music-start.png'"
        style="width: 100%;height: 100%;border-radius: 50%;" />
    </div>



    <div class="isHide" v-if="isHide">
      <view class='content'>
        <view class='header'>
          <image :src='userInfo.avatarUrl'></image>
        </view>
        <view style="text-align: center;">申请获取以下权限</view>
        <text style="text-align: center;">获得你的公开信息(昵称，头像等)</text>
      </view>

      <button class='bottom' type='primary' open-type="getUserInfo" @getuserinfo="getUserInfo">
        授权登录
      </button>
    </div>






    <!-- 轮播图 -->
    <div class="iscontert" v-show="iscontert">
      <view class="page__hd">
        <view class="page__title"></view>
        <view class="page__desc"></view>
      </view>
      <div class="page__bd page__bd_spacing">
        <swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration"
          :circular="circular" @change="swiperChange" @animationfinish="animationfinish">
          <div v-for="item in imgUrls" :key="index">
            <swiper-item>
              <image :src="item" class="slide-image" />
            </swiper-item>
          </div>
        </swiper>
      </div>


      <!-- 内容 -->
      <div class="activity">
        <div class="activity-top">
          <div class="top-1">
            <p>{{hotlist.enroll}}</p>
            <p>已报名</p>
          </div>
          <div class="top-2">
            <p>{{hotlist.sumVote}}</p>
            <p>总投票</p>
          </div>
          <div class="top-3">
            <p>{{hotlist.browse}}</p>
            <p>浏览量</p>
          </div>
        </div>
        <div class="activity-lop" @click="gostore()">
          <p>我要报名</p>
        </div>



        <!-- 倒计时 -->
        <div class="activity-hop">
          <p>{{mags}}</p>
          <!-- <b id="time_b"></b> -->
        </div>





        <div class="activity-bop">
          <input type="text" placeholder="姓名" class="inp" v-model="names">
          <button class="btn" @click="search()">搜索</button>
        </div>
        <div class="grouping-bop">
          <picker class="grouping" @change="bindPickerChange" :value="index" :range="array">
            <view class="picker">
              {{array[index]}}
              <i class="arrowDown"></i>
            </view>
          </picker>
        </div>
        <div class="body">
          <div class="body-right" v-for="(v,index) in namelist" :key="index" @click="goplayer(v.id)">
            <img :src="namelist[index].coverImg" alt="">
            <p style="margin-top: 3%; margin-left: 6%;">{{namelist[index].name}}</p>
            <p style="margin-top: 3%;color: #7CDCDE;margin-left: 6%">{{namelist[index].ticket}}票</p>
            <div class="butt">投票</div>
            <div style="background:black;opacity: 0.4;position: absolute;top: 0;left: 0;">
              <p style="font-size: 12px;color: white;text-align: center;line-height: 26px;">编号:{{namelist[index].id}}</p>
            </div>
          </div>
          <!-- <div class="body-left">
          <img src="namelist[1].coverImg" alt="">
                <p style=" margin-top: 3%; margin-left: 6%;">{{namelist[1].name}}</p>
          <p style="margin-top: 3%;color: #7CDCDE;margin-left: 6%">{{namelist[1].ticket}}票</p>
          <div class="butt">投票</div>
        </div> -->
        </div>
        <div class="foot">
          <p style="text-align: center; line-height: 222rpx;">已经到底部了~</p>
        </div>
      </div>



    </div>

  </div>



</template>
<script>
  import Flys from "../../App.vue"
  export default {
    data() {
      return {
        userInfo: {

        },
        audioCtx: "",
        isMusicPlay: false,
        musicUrl: "http://119.96.181.29/amobile.music.tc.qq.com/C400001L1mv24ORYiL.m4a?guid=9903215364&vkey=706148E1BDDACA45D183C8DE99FD41F14CC4CA6BD5677A7658BA58C6D1540434982B2518B09390D2B7EABB28512199E8B3F68CB0C0934151&uin=2698&fromtag=66",
        isshow: false,
        indicatorDots: true,
        autoplay: true,
        interval: 1000,
        duration: 900,
        circular: true,
        hotlist: "",
        namelist: "",
        names: "",
        time:"",//截止时间
        //判断小程序的API，回调，参数，组件等是否在当前版本可用。
        canIUse: wx.canIUse('button.open-type.getUserInfo'),
        isHide: true,
        iscontert: true,
        imgUrls: ['https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1571993800579&di=3660cdb54cd2a8886189d80bf1ca97ca&imgtype=jpg&src=http%3A%2F%2Fpic.16pic.com%2F00%2F10%2F46%2F16pic_1046407_b.jpg',
          'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1571993790598&di=76f06dfd0956f087f4e36e9315f29aa0&imgtype=0&src=http%3A%2F%2Fwww.gzdexian.com%2Fuploads%2Fimage%2F20180821%2F1534854014.jpg',
          'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2238756297,3469590240&fm=26&gp=0.jpg'
        ],
        array: [
          "选择分组",
          "分组1",
          "分组2",
          "分组3"
        ],
        index: 0,
        days: 1,//天
        hous: 1,//时
        mous: 1,//分
        sed: 1,//秒 
        mags: "",
      }
    },

    onReady(e) {
      this.audioCtx = wx.createAudioContext("myAudio");
      this.handleMusicPlay()
    },

    modalcnt1: function () {

      wx.startPullDownRefresh()
    },

    modalcnt2: function () {

      wx.stopPullDownRefresh()
    },
    

    //下拉刷新
    onPullDownRefresh: function () {
      console.log("刷新");
      this.activityPlayer()
      wx.hideNavigationBarLoading() //完成停止加载
      wx.stopPullDownRefresh() //停止下拉刷新

    },
    onLoad() {
      
      this.list()
      this.activityPlayer()
      var that = this
      wx.getUserInfo({
        success: (data) => {
          this.isHide = false
          this.isshow = true
          console.log(data)
          wx.login({
            success(res) {
              console.log(res, "成功回调")
              if (res.code) {
                //发起网络请求
                that.$fly.post(Flys.getAppid, { code: res.code }).then(res => {
                  console.log(res, "后台返回的数据")
                  
                })
              } else {
                console.log('登录失败！' + res.errMsg)
              }
            }
          })
        },
        fail: () => {
          console.log('获取失败')
          wx.hideTabBar({
            success: (res => {
              // console.log(11111111)  
            })
          })
        }
      })
    },
    beforeMount() {
      //获取用户登录信息
      this.handLeGetUserInfo()
    },
    methods: {
      handleMusicPlay() {
        let isMusicPlay = this.isMusicPlay;
        this.isMusicPlay = isMusicPlay;
        console.log(1111111)
        if (this.isMusicPlay) {
          this.audioCtx.pause();
          this.isMusicPlay = false
        } else {
          this.audioCtx.play();
          this.isMusicPlay = true
        }
      },


      //获取用户登录信息
      handLeGetUserInfo() {
        wx.getUserInfo({
          success: (data) => {

            console.log(data)
            //更新data中的数据
            this.userInfo = data.userInfo
          },
          fail: () => {
            wx.hideTabBar({
              success: (res => {
                // console.log(11111111)  
              })
            })
            // console.log('获取失败')
          }
        })
      },
      getUserInfo(data) {
        // console.log(data)
        //判断用户是否授权
        if (data.mp.detail.rawData) {
          //用户授权
          this.handLeGetUserInfo()
          this.isHide = false
          wx.showTabBar()
          this.isshow = true
        } else {
          wx.showModal({
            title: "警告",
            content: '您点击了拒绝授权，将无法进入小程序，请授权之后再进入！！！',
            showCancel: false,
            confirmText: '返回授权',
          })
        }
        
      },
      bindPickerChange(e) {
        this.index = e.mp.detail.value;
      },
      goplayer(id) {
        console.log(id)
        wx.navigateTo({
          url: "../player/main?id="+id

        })
      },
      gostore() {
        // console.log(111)
        wx.navigateTo({
          url: "../Store/main"
        })
      },
      list() {
        this.$fly.post(Flys.list, { id: 1 }).then(res => {
          console.log(res, "活动详情页")
          this.hotlist = res.data.data.hdActivity
          this.time = res.data.data.hdActivity.end
          this.conuttiem()
        })
      },
      activityPlayer() {
        this.$fly.post(Flys.activityPlayer, { activityId: 1 }).then(res => {
          console.log(res, "活动详情页选手列表")
          this.namelist = res.data.rows
        })
      },


      conuttiem() {
        var timeid;
        var date = new Date();
        var now = date.getTime();
        var endDate = new Date(this.time)//设置时间
        var end = endDate.getTime();
        var leftTime = end - now;//时间差
        if (leftTime >= 0) {
          this.days = Math.floor(leftTime / 1000 / 60 / 60 / 24);
          this.hous = Math.floor(leftTime / 1000 / 60 / 60 % 24);
          this.mous = Math.floor(leftTime / 1000 / 60 % 60);
          this.sed = Math.floor(leftTime / 1000 % 60);
          this.hous = this.hous < 10 ? "0" + this.hous : this.hous;
          this.mous = this.mous < 10 ? "0" + this.mous : this.mous
          this.sed = this.sed < 10 ? "0" + this.sed : this.sed
          this.mags = "距离活动结束:" + this.days + "天" + this.hous + "时" + this.mous + "分" + this.sed + "秒"
          timeid = setInterval(this.conuttiem, 1000);
        } else {
          this.mags = "活动已结束";
          clearInterval(timeid)
        }
      },
      search() {
        if (this.names == "") {
          wx.showToast({
            title: '请输入姓名',
            icon: "none",
            duration: 2000
          })
        } else {
          this.$fly.post(Flys.activityPlayer, { name: this.names }).then(res => {
            console.log(res, "1111111111")  
            this.namelist = res.data.rows
          })
        }

        this.names = ""

      }
    }
  }
</script>
<style scoped>
  .slide-image {
    width: 100%;
    height: 100%;
  }

  .page {
    width: 100%;
    height: 100%;
  }

  .activity {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    background: #EEEEEE;
  }

  .activity-top {
    display: flex;
    flex-direction: row;
    width: 94%;
    height: 125rpx;
    background: #31c9b1;
    margin: 0 auto;
    margin-top: 4%;
    border-radius: 5rpx;
  }

  .top-1,
  .top-2,
  .top-3 {
    display: flex;
    flex-direction: column;
    text-align: center;
    align-items: center;
    justify-content: center;
    width: 31%;
    font-size: 16px;
    /* line-height: 40rpx; */
    color: white;
  }

  .activity-lop {
    display: flex;
    flex-direction: column;
    width: 70%;
    height: 80rpx;
    background: #31c9b1;
    color: white;
    justify-content: center;
    text-align: center;
    /* font-size: 16px */
    margin: 0 auto;
    margin-top: 5%;
    border-radius: 25rpx;
  }

  .activity-hop {
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    width: 94%;
    height: 70rpx;
    background: #f6ffff;
    margin: 0 auto;
    margin-top: 5%;
    color: #8F97AD;
    font-size: 35rpx
  }

  .activity-bop {
    display: flex;
    flex-direction: row;
    width: 94%;
    height: 80rpx;
    margin: 0 auto;
    margin-top: 5%;
  }

  .inp {
    width: 66%;
    height: 80rpx;
    background: white;
    /* text-indent: 5%; */
  }

  .btn {
    width: 34%;
    height: 80rpx;
    background: #31C9B1;
    line-height: 80rpx;
    text-align: center;
    color: white;
    border-radius: 0;
  }

  .btn::after {
    border-radius: 0;
  }

  .grouping-bop {
    width: 30%;
    height: 80rpx;
    background: #31c9b1;
    font-size: 35rpx;
    color: white;
    margin-top: 3%;
    text-align: center;
    line-height: 80rpx;
    margin-left: 3%;
  }

  .body {
    display: flex;
    /* flex-direction: row; */
    justify-content: space-between;
    flex-wrap: wrap;
    width: 94%;
    height: 100%rpx;
    /* background: white; */
    margin: 0 auto;
    margin-top: 3%;
  }

  .body-right {
    display: flex;
    flex-direction: column;
    width: 47%;
    height: 584rpx;
    /* border: 1rpx solid red; */
    background: white;
    margin-bottom: 3%;
    position: relative;
  }

  .body-left {
    display: flex;
    flex-direction: column;
    width: 47%;
    height: 584rpx;
    /* border: 1rpx solid red; */
    background: white;
    margin-left: 6%;
  }

  img {
    width: 100%;
    height: 100%;
    /* margin-left: 6%; */
  }

  .butt {
    width: 88%;
    height: 100rpx;
    background: #31C9B1;
    color: white;
    margin: 0 auto;
    margin-top: 3%;
    margin-bottom: 3%;
    text-align: center;
    line-height: 57rpx;
  }

  .foot {
    width: 94%;
    height: 222rpx;
    /* background: #31C9B1; */
    margin: 0 auto;
    color: #D6D2D2;
  }

  .header {
    margin: 90rpx 0 90rpx 50rpx;
    border-bottom: 1px solid #ccc;
    text-align: center;
    width: 650rpx;
    height: 300rpx;
    line-height: 450rpx;
  }

  .header image {
    width: 200rpx;
    height: 200rpx;
  }

  .content {
    /* margin-left: 50rpx;
    margin-bottom: 90rpx; */
    z-index: 99;
  }

  .content text {
    display: block;
    color: #9d9d9d;
    margin-top: 40rpx;
  }

  .bottom {
    border-radius: 80rpx;
    margin: 70rpx 50rpx;
    font-size: 35rpx;
  }

  .isHide {
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background: white;
    z-index: 99;
  }

  .music {
    position: fixed;
    right: 4%;
    top: 6%;
    z-index: 999;

  }
</style>