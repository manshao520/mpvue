<template>
    <div class="page">
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
      <div class="header">
          热门活动
      </div>
      <div class="foot">
          <div class="foot-top" @click="gohome()">
            <p style="width: 12%;color: red;height: 100rpx;line-height: 100rpx;text-indent: 21rpx">1</p>
            <p style="width: 39%;font-weight: 700;height: 100rpx;line-height: 100rpx;white-space: nowrap;overflow: hidden;text-overflow: ellipsis;-o-text-overflow: ellipsis;">{{hotlist[0].name}}</p>
            <p style="width: 35%;opacity: 0.5;height: 100rpx;line-height: 100rpx">{{hotlist[0].browse}}</p>
            <p style="width: 14%;height: 100rpx;line-height: 100rpx">火</p>
          </div>
          <view class="divLine"></view>
          <!-- <div class="foot-bop">
            <p style="width: 12%;color: red;height: 100rpx;line-height: 100rpx;text-indent: 21rpx">2</p>
            <p style="width: 39%;font-weight: 700;height: 100rpx;line-height: 100rpx;white-space: nowrap;overflow: hidden;text-overflow: ellipsis;-o-text-overflow: ellipsis;">{{hotlist[1].name}}</p>
            <p style="width: 35%;opacity: 0.5;height: 100rpx;line-height: 100rpx">{{hotlist[1].browse}}</p>
            <p style="width: 14%;height: 100rpx;line-height: 100rpx">火</p>
          </div> -->
      </div>
    </div>
  
</template>

<script>
  import Flys from "../../App.vue"
export default { 
        data() { 
            return { 
                indicatorDots: true, 
                autoplay: true, 
                interval: 1000, 
                duration: 900, 
                circular: true, 
                hotlist:"",
                name:"",
                imgUrls: ['https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1571993800579&di=3660cdb54cd2a8886189d80bf1ca97ca&imgtype=jpg&src=http%3A%2F%2Fpic.16pic.com%2F00%2F10%2F46%2F16pic_1046407_b.jpg', 
                'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1571993790598&di=76f06dfd0956f087f4e36e9315f29aa0&imgtype=0&src=http%3A%2F%2Fwww.gzdexian.com%2Fuploads%2Fimage%2F20180821%2F1534854014.jpg', 
                'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2238756297,3469590240&fm=26&gp=0.jpg'
            ] 
        } 
    },
    onLoad:function(){
      this.getActivityList();
    },
    
    methods: {
          gohome(){
            // console.log(1111)
            wx.switchTab({
              url:"../index/main"
            })
            wx.switchTab({
              url:"../index/main"
            })
          },
          getActivityList(){
            this.$fly.post(Flys.getActivityList).then(res=>{
              console.log(res,"活动页")
              this.name=res.data.data[0].name
                this.hotlist = res.data.data
            })
          }
        }
}
</script>

<style scoped>
  .slide-image {
        width: 100%;
        height: 100%;
    }
  .header{
    display: flex;
    width: 100%;
    height: 80rpx;
    /* border: 1rpx solid red; */
    line-height: 95rpx;
    font-weight: 700;
    text-indent: 21rpx;
    font-size: 40rpx;
  }
  .foot{
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 200rpx;
    /* border: 1rpx solid red; */
  }
  .foot-top{
    display: flex;
    flex-direction: row;
    width: 100%;
    height: 100rpx;
    border-top: 5rpx solid #F2F2F2
  }
  .foot-bop{
    display: flex;
    flex-direction: row;
    width: 100%;
    height: 100rpx;
    border-bottom:5rpx solid #F2F2F2;
  }
  .divLine{
    background:#F2F2F2;
    width: 100%;
    height: 5rpx;
  }
</style>