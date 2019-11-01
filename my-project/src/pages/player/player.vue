<template>
    <div class="body">
        <div class="header">
            <div class="header-pho">
                <img :src="playername.HdPlayer.coverImg">
            </div>
            <div class="header-text">
                <p style="text-align: center;">{{playername.HdPlayer.name}}</p>
            </div>
        </div>
        <div class="part">
            <div>
                <p>{{playername.HdPlayer.i}}</p>
                <p>排行</p>
            </div>
            <div>
                <p>{{playername.HdPlayer.ticket}}</p>
                <p>票数</p>
            </div>
            <div>
                <p>{{playername.HdPlayer.gift}}</p>
                <p>礼物</p>
            </div>
            <div>
                <p>{{playername.HdPlayer.browse}}</p>
                <p>浏览量</p>
            </div>
        </div>
        <div class="describe">
            <div class="describe-top">
                <p>选手描述</p>
            </div>
            <div class="describe-bop">
                <p>一杯茶，一包烟，一行代码敲一天</p>
            </div>
        </div>
        <div class="photo">
            <div class="photo-top">
                <p>选手照片</p>
            </div>
            <div class="photo-bop">
                <img :src="playername.playerImg[0].url" alt="">
            </div>
        </div>
        <div class="list">
            <div class="list-top">
                <p>票数贡献榜</p>
            </div>
            <div class="list-bop">
                <img src="../../../static/images/user.png" alt="">
                <p style="color: #CDD7DA; margin: 30rpx auto;">喜欢我的人在这里驻留</p>
                <div
                    style="width: 65%; height: 100rpx ;background: #31C9B1;color: white;text-align: center; line-height:100rpx; border-radius: 50rpx;">
                    送ta礼物加票</div>
            </div>
        </div>
        <div class="vote">
            <div class="vote-top">
                <p>投票记录</p>
            </div>
            <div class="vote-bop">
                <div class="btn">
                    <p>已经是最后一页了</p>
                </div>
            </div>
        </div>

        <!-- 底部导航 -->
        <div class="foot">
            <div class="foot-left">
                <p><i class="icon iconfont icon-shouye2"></i></p>
                <p><i>首页</i></p>
            </div>
            <div class="foot-middle">
                <p><i class="icon iconfont icon-xiaoyuantoupiao"></i></p>
                <p>投票</p>
            </div>
            <div class="foot-right">
                <p><i class="icon iconfont icon-lipin"></i></p>
                <p @click=gogift(this.playerid)><i>送礼</i></p>
            </div>
        </div>
    </div>
</template>

<script>
    import Flys from "../../App.vue"
    export default {
        data(){
           return{
                playername:"",
                playerid:""
           } 
        },
        onLoad(options){
            console.log(options,"1111111")
            this.playerid = options.id
            this.player()
        },
        methods: {
            player() {
                this.$fly.post(Flys.player, { id:this.playerid }).then(res => {
                    console.log(res, "选手详情")
                    this.playername = res.data.data//第一个值

                })
            },
            gogift(playerid) {
                console.log(this.playerid)
                wx.navigateTo({
                    url: "../gift/main?id="+this.playerid
                })
            }
        }
    }
</script>

<style scoped>
    .body {
        background: #F0F0F0;
        width: 100%;
        height: 2600rpx;
    }

    .header {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 380rpx;
    }

    .header-pho {
        overflow: hidden;
        width: 200rpx;
        height: 200rpx;
        border-radius: 50%;
        /* align-content: center; */
    }

    .header-pho img {
        width: 100%;
        height: 100%;
    }

    .header-text {
        width: 120rpx;
        height: 100%rpx;
        color: #9FABBB;
        margin-top: 2%;
    }

    .part {
        display: flex;
        flex-direction: row;
        margin: 0 auto;
        width: 92%;
        height: 150rpx;
        background: #31C9B1;
        color: white;
        border-radius: 5rpx;
    }

    .part div {
        width: 25%;
        height: 150rpx;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .describe {
        display: flex;
        flex-direction: column;
        width: 92%;
        height: 222rpx;
        background: white;
        margin: 42rpx auto;
    }

    .describe-top {
        width: 100%;
        height: 30%;
        border-bottom: 1rpx solid #F6F6F6;
        text-align: center;
        line-height: 66rpx;
        color: #CDD7DA;
    }

    .describe-bop {
        width: 100%;
        height: 70%;
        color: #CDD7DA;
    }

    .photo {
        display: flex;
        flex-direction: column;
        width: 92%;
        height: 700rpx;
        background: white;
        margin: 0 auto;
    }

    .photo-top {
        width: 100%;
        height: 10%;
        border-bottom: 1rpx solid #F6F6F6;
        /* background: yellow; */
        text-align: center;
        line-height: 77rpx;
        color: #CDD7DA;
    }

    .photo-bop {
        display: flex;
        width: 100%;
        height: 90%;
        justify-content: center;
        /* background: yellow; */
    }

    .photo-bop img {
        width: 90%;
        height: 85%;
        margin-top: 5%;
    }

    .list {
        display: flex;
        flex-direction: column;
        width: 92%;
        height: 580rpx;
        background: white;
        margin: 42rpx auto;
    }

    .list-top {
        width: 100%;
        height: 10%;
        border-bottom: 1rpx solid #F6F6F6;
        /* background: yellow; */
        text-align: center;
        line-height: 70rpx;
        color: #CDD7DA;
    }

    .list-bop {
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 630rpx;
        /* justify-content: center; */
        align-items: center;
    }

    .list-bop img {
        width: 50%;
        height: 50%;
        /* margin-top: 0; */
    }

    .vote {
        display: flex;
        flex-direction: column;
        width: 92%;
        height: 250rpx;
        background: white;
        margin: 42rpx auto;
    }

    .vote-top {
        width: 100%;
        height: 30%;
        border-bottom: 1rpx solid #F6F6F6;
        text-align: center;
        line-height: 66rpx;
        color: #CDD7DA;
    }

    .vote-bop {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 166rpx;
    }

    .btn {
        width: 94%;
        height: 91rpx;
        background: #31C9B1;
        border-radius: 5rpx;
        color: white;
        text-align: center;
        line-height: 91rpx;
    }

    /* .foot{
        width: 100%;
        height: 150rpx;
        display: flex;
        /* background: red; */
    /* flex-direction: row;
        position: fixed;
        bottom: 0;
        justify-content: space-between;
        background: red; */
    /* } */
    .foot {
        width: 100%;
        height: 90rpx;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        position: fixed;
        bottom: 0;
        background: white;
    }

    .foot-left {
        display: flex;
        flex-direction: row;
        background-color: #9EE786;
        width: 33%;
    }

    .foot-right p:nth-child(1) i,
    .foot-left p:nth-child(1) i {
        font-size: 60rpx;
        color: #31C9B1;
        position: relative;
        top: 15%;
        left: 50%;
    }

    .foot-right p:nth-child(2) i,
    .foot-left p:nth-child(2) i {
        font-size: 16px;
        position: relative;
        top: 30%;
        left: 60%;
    }

    .foot-middle {
        display: flex;
        flex-direction: column;
        background-color: #9EE786;
        width: 33%;
    }

    .foot-middle p:nth-child(1) i {
        font-size: 80rpx;
        position: relative;
        top: -55%;
        left: 30%;
        color: #08C960;
        background-color: white;
        width: 90rpx;
        padding-left: 10rpx;
        border-radius: 50%;
    }

    .foot-middle p:nth-child(2) {
        width: 90rpx;
        height: 20rpx;
        font-size: 15px;
        text-align: center;
        position: relative;
        top: -45%;
        left: 30%;

    }

    .foot-right {
        display: flex;
        flex-direction: row;
        background-color: #9EE786;
        width: 33%;
    }

    .foot-right p:nth-child(1) i {
        font-size: 30px;
        color: #31C9B1;
    }
</style>