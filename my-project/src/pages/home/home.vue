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


        
        <div class="body">
          <div class="body-top">
            <div class="body-top-a" @click="gologs()">
              <img src="../../../static/images/85EBD0E967CA30173AD8F3D50D0BF061.jpg" alt="">
              <p style="text-align: center">活动</p>
            </div>
            <div class="body-top-b" @click="goquestionnaire()">
              <img src="../../../static/images/85EBD0E967CA30173AD8F3D50D0BF061.jpg" alt="">
              <p style="text-align: center">问卷</p>
            </div>
          </div>
          <div class="body-bop">
            <div class="body-bop-a">
              <img src="../../../static/images/85EBD0E967CA30173AD8F3D50D0BF061.jpg" alt="">
              <p style="text-align: center">夏令营</p>
            </div>
            <div class="body-bop-b">
              <img src="../../../static/images/85EBD0E967CA30173AD8F3D50D0BF061.jpg" alt="">
              <p style="text-align: center">课程</p>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
export default { 
        data() { 
            return { 
                indicatorDots: true, 
                autoplay: true, 
                interval: 1000, 
                duration: 900, 
                circular: true, 
                imgUrls: ['https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1571993800579&di=3660cdb54cd2a8886189d80bf1ca97ca&imgtype=jpg&src=http%3A%2F%2Fpic.16pic.com%2F00%2F10%2F46%2F16pic_1046407_b.jpg', 
                'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1571993790598&di=76f06dfd0956f087f4e36e9315f29aa0&imgtype=0&src=http%3A%2F%2Fwww.gzdexian.com%2Fuploads%2Fimage%2F20180821%2F1534854014.jpg', 
                'https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=2238756297,3469590240&fm=26&gp=0.jpg'
            ] 
        } 
    },
    methods: {
          gologs(){
            // console.log(1111)
            wx.navigateTo({
              url:"../logs/main"
            })
          },
          goquestionnaire(){
            // console.log(111)
            wx.navigateTo({
              url:"../questionnaire/main"
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
  .body{
    display: flex;
    /* justify-content: space-between; */
    /* border: 1rpx solid red; */
    flex-direction: column;
    width: 100%;
    height: 100%;
    background:#EEEEEE;
  }
  .body-top{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin: 0 auto;
    width: 96%;
    height: 300rpx;
    /* background: #F6FFFF; */
    margin-top: 5%;
  }
  .body-top-a{
    display: flex;
    flex-direction: column;
    width: 48%;
    height: 300rpx;
    background: #F6FFFF;
    border: solid 2px #E0F7FA;
  }
  .body-top-b{
    display: flex;
    flex-direction: column;
    width: 48%;
    height: 300rpx;
    background: #F6FFFF;
    border: solid 2px #E0F7FA;
  }
  img{
    width: 90%;
    height: 100%;
    margin: 0 auto;
    margin-top: 5%;
  }
  .body-bop{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin: 0 auto;
    width: 96%;
    height: 100%;
    /* background: #F6FFFF; */
    margin-top: 2%;
  }
  .body-bop-a{
    display: flex;
    flex-direction: column;
    width: 48%;
    height: 300rpx;
    background: #F6FFFF;
    border: solid 2px #E0F7FA;
  }
  .body-bop-b{
    display: flex;
    flex-direction: column;
    width: 48%;
    height: 300rpx;
    background: #F6FFFF;
    border: solid 2px #E0F7FA;
  }
</style>