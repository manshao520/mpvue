<template>
    <div class="body">
        <div class="header">
            <div class="header-pho">
                <img :src="playername.HdPlayer.coverImg" alt="">
            </div>
            <div class="Header-text">
                <p>{{playername.HdPlayer.name}}</p>
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
        <div class="gift">
            <div class="gift-head">
                <p style="color:#899098 ;font-size: 14px;line-height: 90rpx;text-indent: 3%;">送礼物加票数</p>
            </div>
            <div class="gift-body">
                <div class="sumbox" v-for="(v,index) in items" :key="index" @click="mama(index)"
                    :class="flag==index?'sumbox':'baba'">
                    <div style="color: #BBC0C6;">{{v.name}}</div>
                    <div><img :src="v.icon" alt=""></div>
                    <div class="addtokey">+{{v.ticket}}票</div>

                </div>
            </div>
        </div>
        <div class="gift-foot">
            <p style="line-height: 120rpx;">￥{{total}}+{{votes}}票</p>
            <div style="display: flex;align-items: center;margin-right: 2%;">
                <div style="width: 60rpx;height: 60rpx;border: 1rpx solid black;text-align: center;line-height: 60rpx;"
                    @click="bdd()">
                    -
                </div>
                <p style="width: 150rpx;height: 60rpx;text-align: center;line-height: 60rpx;border: 1rpx solid black;">
                    {{num}}
                </p>
                <div style="width: 60rpx;height: 60rpx;border: 1rpx solid black;text-align: center;line-height: 60rpx;"
                    @click="add()">
                    +</div>
            </div>
        </div>
        <button style="width: 100%;background: #1ABA0E;color: white;">立即送出</button>
    </div>

</template>

<script>
    import Flys from "../../App.vue"
    export default {
        data() {
            return {
                items: [
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 20, money: 9 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 21, money: 1 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 22, money: 2 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 23, money: 3 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 24, money: 4 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 25, money: 5 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 26, money: 6 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 27, money: 7 },
                    { name: "孙悟空", icon: "../../static/images/baozang.jpg", ticket: 28, money: 8 }
                ],
                flag: 0,//对比数组下标标识
                vote: 20,
                num: 1,
                price: 0.01,
                total: 0.01,
                votes: 20,
                playername:""
            }
        },
        onLoad(options) {
            console.log(options, 2222222222)
            this.playerid = options.id
            console.log(this.playerid, "1111111")
            this.giftList()
            this.player()

        },
        methods: {
            giftList() {
                this.$fly.post(Flys.giftList).then(res => {
                    console.log(res, "礼物页面")
                    this.items = res.data.data.hdGifts
                })
            },
            player() {
                this.$fly.post(Flys.player, { id: this.playerid }).then(res => {
                    console.log(res, "选手详情")
                    this.playername = res.data.data//第一个值

                })
            },
            mama(index) {
                this.vote = this.items[index].ticket
                this.price = this.items[index].price
                // console.log(this.items[index].ticket)          
                this.flag = index
                this.num = 1
                this.total = this.price
                this.votes = this.vote

            },
            add(index) {
                if (this.num >= 99) {
                    this.num = 98
                }
                this.num++
                this.total = this.num * this.price
                this.votes = this.num * this.vote
                this.total = this.total.toFixed(2)

            },
            bdd(index) {

                if (this.num <= 1) {
                    wx.showModal({
                        title: "提示！",
                        content: "礼物数量不能小于1",
                        showCancel: false
                    })
                    this.num = 1

                } else {
                    this.num--
                    this.total = this.total - this.price
                    this.votes = this.votes - this.vote
                    this.total = this.total.toFixed(2)
                }





            }
        }
    }
</script>

<style scoped>
    .body {
        background: #E6E6E6;
        width: 100%;
        /* height: 2600rpx; */
        display: flex;
        flex-direction: column;
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
        width: 100%;
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

    .gift {
        width: 100%;
        height: 795rpx;
        background: #E6E6E6;
        display: flex;
        flex-direction: column;
        margin-top: 6%;
        align-items: center;
    }

    .gift-head {
        width: 96%;
        height: 90rpx;
        background: white;
    }

    .gift-body {
        width: 94%;
        margin: 0 auto;
        /* border: 1px solid red; */
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
        background: #F0F0F0;
    }

    .sumbox {
        width: 30%;
        height: 200rpx;

        margin-bottom: 10px;
        text-align: center;
        position: relative;
        background: gainsboro;
        margin-top: 2%;
    }

    .sumbox img {
        width: 50px;
        height: 50px;
    }

    .addtokey {
        background-color: #B3E757;
        color: #839A86;
        position: absolute;
        bottom: 0;
        width: 100%;
    }

    .gift-foot {
        width: 94%;
        display: flex;
        /* height: 120rpx; */
        /* border: 1rpx solid red; */
        margin: 0 auto;
        justify-content: space-between;
        flex-wrap: wrap;
        background: #F0F0F0;
    }

    .baba {
        width: 30%;
        height: 200rpx;
        margin-bottom: 10px;
        text-align: center;
        position: relative;
        background: white;
        margin-top: 2%;
    }
</style>