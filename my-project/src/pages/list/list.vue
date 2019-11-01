<template>
    <div class="body">
        <div class="header">
            <p>2019-10-31 00:00:00 结束</p>
            <p>{{mags}}</p>
        </div>
        <div class="foot">
            <div class="foot-top">
                <p>票数排行榜</p>
            </div>
            <div class="foot-bop" :key="index">
                <div class="foot-bop-a">
                    <p style="color: #F9CA4F;margin-bottom:5%;">NO.2</p>
                    <p style="width: 140rpx;height: 140rpx;">
                        <img :src="hotlist[1].coverImg" alt="" style="width: 100%;height: 100%;border-radius: 50%;">
                    </p>
                    <p style="color: #A1AAB2; margin: 10rpx;">{{hotlist[1].name}}</p>
                    <p style="color: #57C3AA;margin: 10rpx;">{{hotlist[1].ticket}}票</p>
                </div>
                <div class="foot-bop-b">
                    <p style="width: 182rpx;height: 74rpx;">
                        <img src="../../../static/images/QQ截图20191012142805.png" alt=""
                            style="width: 100%;height: 100%;position: relative;top: -20%;">
                    </p>
                    <p style="width: 160rpx;height: 160rpx;">
                        <img :src="hotlist[0].coverImg" alt=""
                            style="width: 100%;height: 100%;border-radius: 50%;position: relative;top: -18%;">
                    </p>
                    <p style="color: #768696;font-size: 20px;font-weight: 700;">{{hotlist[0].name}}</p>
                    <p style="color:#57C3AA ;font-weight: 700;font-size: 23px;">{{hotlist[0].ticket}}票</p>
                </div>
                <div class="foot-bop-a">
                    <p style="color: #F9CA4F;margin-bottom:5%;">NO.3</p>
                    <p style="width: 140rpx;height: 140rpx;">
                        <img :src="hotlist[2].coverImg" alt="" style="width: 100%;height: 100%;border-radius: 50%;">
                    </p>
                    <p style="color: #A1AAB2; margin: 10rpx;">{{hotlist[2].name}}</p>
                    <p style="color: #57C3AA;margin: 10rpx;">{{hotlist[2].ticket}}票</p>
                </div>
            </div>
            <div class="divLine"></div>


            <div class="foot-dop">
                <div class="foot-dop-a" @click="gogift()" v-for="(v,index) in arre" :key="index">
                    <p style="width: 30%;height: 100%;display: flex;justify-content: center;align-items: center;">
                        <img :src="v.coverImg" alt="" style="width: 60%;height: 75%;border-radius: 50%;">
                    </p>
                    <div
                        style="width: 50%;display: flex;flex-direction: column;justify-content: center;color: #969FA8;">
                        <p>{{v.name}}</p>
                        <p>{{v.ticket}}票</p>
                    </div>
                    <p class="paiming">{{index+4}}</p>
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
                days: 1,//天
                hous: 1,//时
                mous: 1,//分
                sed: 1,//秒 
                mags: "",
                hotlist: "",
                arrs: [],
                arre: [],
                
            }
        },
        created() {
            this.conuttiem()
            this.playerRank()
        },
        methods: {
            gogift() {
                // console.log(1111)
                wx.navigateTo({
                    url: "../gift/main"
                })
            },
            playerRank() {
                this.$fly.post(Flys.playerRank, { activityId: 1 }).then(res => {
                    console.log(res, "排行榜")
                    this.hotlist = res.data.data
                    for (var i = 0; i < this.hotlist.length; i++) {
                    
                        if (i < 3) {
                            this.arrs.push(this.hotlist[i])
                        } else {
                            this.arre.push(this.hotlist[i])
                        }
                    }
                    console.log(this.arrs, "排行榜前三名")
                    console.log(this.arre, "排行榜后七名")
                })
            },

            conuttiem() {
                var timeid;
                var date = new Date();
                var now = date.getTime();
                var endDate = new Date("2019-10-31 00:00:00")//设置时间
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
            }
        }
    }

</script>

<style scoped>
    .body {
        width: 100%;
        height: 100%;
        background: #EEEEEE;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .header {
        width: 100%;
        height: 100%;
        /* background: white; */
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .header p {
        margin: 6rpx auto;
        color: #999FA4;
        font-size: 13px;
    }

    .foot {
        width: 94%;
        height: 100%;
        background: white;
    }

    .foot-top {
        width: 100%;
        height: 95rpx;
        background: #31C9B2;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        font-size: 15px;
    }

    .foot-bop {
        width: 100%;
        height: 400rpx;
        /* background: red; */
        display: flex;
        flex-direction: row;
    }

    .foot-bop-a,
    .foot-bop-b,
    .foot-bop-c {
        height: 400rpx;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .foot-bop-a {
        width: 35%;
    }

    .foot-bop-b {
        width: 30%;
    }

    .foot-bop-c {
        width: 35%;
    }

    .divLine {
        background: #E0E3DA;
        width: 100%;
        height: 3rpx;
    }

    .foot-dop {
        width: 100%;
        height: 100%;
        /* background: red; */
    }

    .foot-dop-a {
        display: flex;
        flex-direction: row;
        width: 100%;
        height: 150rpx;
        /* background: red; */
        border-bottom: 1rpx solid gainsboro;
        ;
    }

    .paiming {
        height: 75px;
        width: 87rpx;
        line-height:75px ;
        text-align: center;
        /* border: 1px solid red; */
    }
</style>