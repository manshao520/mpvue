<template>
    <div class="baoming">
        <div class="baoming-zaixian">
            <p>在线报名</p>
        </div>
        <div class="baoming-xinxi">
            <div class="xinxi-name">
                <p>选手名称</p>
                <p><input type="text" placeholder="请输入选手名称" v-model="userName"></p>
            </div>
            <div class="xinxi-phone">
                <p>手机号</p>
                <p><input type="number" placeholder="请输入手机号" v-model="userPhone" maxlength="11"></p>
            </div>
            <div class="xinxi-fenzu">
                <p>分组</p>
                <p>
                    <picker class="grouping" @change="bindPickerChange" :value="index" :range="array">
                        <view class="picker">
                            {{array[index]}}
                            <i class="arrowDown"></i>
                        </view>
                    </picker>
                </p>

            </div>
            <div class="xinxi-miaoshu">
                <p>选手描述</p>
                <p><textarea placeholder="请输入选手描述" v-model="userDescribe"></textarea></p>
            </div>
        </div>
        <p class="Uname" style="width:100%;margin-top:5%;color:black">上传选手图片(1-3张)</p>

        <!-- 图片上传 -->
        <div class="weui-uploader__bd th-backwhite clearfix">
            <div class="weui-uploader__files" id="uploaderFiles" style="width: 100%;height: 100%;margin-bottom: 2%;margin-top: 2%;">
                <block v-for="(item,index) in files" :key="index">
                    <div class="weui-uploader__file posi-rela" @click="predivImage" :id="item">
                        <icon type="cancel" size="20" class="th-icon-cancel" @click.stop="deletImg(index)" />
                        <image class="weui-uploader__img" :src="item" mode="aspectFill"
                            style="width: 100%;height: 100%;" />
                    </div>
                </block>
                <div class="weui-uploader__input-box" style="width: 150rpx;height: 150rpx;margin-left: 20rpx;"
                    v-if="show">
                    <div class="weui-uploader__input" @click="chooseImage" style="width: 100%;height: 100%;">+</div>
                </div>
            </div>

        </div>
        <div class="th-submit-btn" @click="btnSubmission">提交</div>

    </div>

</template>

<script>
    import Flys from "../../App.vue"
    export default {
        data() {
            return {
                hotlist: "",
                array: [
                    "全部 ∨",
                    "分组1",
                    "分组2",
                    "分组3"
                ],
                index: 0,
                //用户名
                userName: "",
                //手机号
                userPhone: "",
                //默认下标为0
                indexs: "0",
                //选手描述
                userDescribe: "",
                //后台传至bs64
                base64: [],
                //判断分组
                grouping: false,
                //添加图片按钮
                show: true,


                files: [],
                filesOnline: []
            }
        },
        created() {
            this.add();
        },

        methods: {
            //分组方法
            bindPickerChange(e) {
                this.index = e.mp.detail.value;
                console.log(this.index,"-------")
            },
            add() {
                this.$fly.post(Flys.add, { activity_id: 1 }).then(res => {
                    console.log(res, "选手保存")
                    this.hotlist = res.data.data
                })
            },
            chooseImage(e) {
                var _this = this;
                wx.chooseImage({
                    count: 1,
                    sizeType: ['original', 'compressed'],
                    sourceType: ['album', 'camera'],
                    success: function (res) {
                        let base64 = wx .getFileSystemManager().readFileSync(res.tempFilePaths[0], "base64");
                           console.log(base64,"base64格式")
                        res.tempFilePaths.forEach(v => {
                            _this.files.push(v);
                            _this.base64.push({ img: "data:image/png;base64," + base64 })
                        });
                    

                        if (_this.files.length < 3) {
                            _this.show = true;
                        } else {
                            wx.showModal({
                                title: "提示！",
                                content: "照片最多上传三张！",
                                showCancel:false
                            })
                            _this.show = false
                        }
                        // console.log(res,"---")
                    }
                })
            },
            predivImage(e) {
                console.log(e, "图片信息");
                wx.previewImage({
                    current: e.currentTarget.id, // 当前显示图片的http链接
                    urls: this.files // 需要预览的图片http链接列表
                })
            },
            deletImg(index) {
                this.files.splice(index, 1)
                this.filesOnline.splice(index, 1)
                if (this.files.length < 3) {
                    this.show = true;
                }
            },
            //提交按钮
            btnSubmission() {
                let slef = this;
                let newBase64 = JSON.stringify(slef.base64);
                    // console.log(newBase64,"nanggnrjgn")
                // console.log(this.userInfo + "----")
                
                let phonetel = /^(((13[0-9]{1})|(15[0-9]{1})|(18[0-9]{1})|(17[0-9]{1}))+\d{8})$/;
                var extend1;
                if(this.userInfo == 1){
                    extend1 == 0;
                }else{
                    extend1 == 1;
                }
                console.log(extend1,'extend1');
                if(slef.userName == ""){
                    wx.showToast({
                        title:'用户不能为空',
                        icon:"none",
                        duration:1500
                    });
                }else if(slef.userPhone == ""){
                    wx.showToast({
                        title:"手机号码不能为空",
                        icon:"none",
                        duration:1500
                    });
                }else if(!phonetel.test(slef.userPhone)){
                    wx.showToast({
                        title:"手机号码格式不对！",
                        icon:"none",
                        duration:1500
                    });
                }else if(this.index == 0){
                    wx.showToast({
                        title:"分组不能为空！",
                        icon:"none",
                        duration:1500
                    })
                }
                else if(slef.base64 == ""){
                    wx.showToast({
                        title:"图片不能为空",
                        icon:"none",
                        duration:1500
                    });
                }else{
                    wx.showLoading({
                        title:"正在提交···",
                        duration:1500
                    });
                   
                   this.jiekou

                }
            }
        },
    }


</script>


<style scoped>
    * {
        margin: 0;
        padding: 0;
    }

    .baoming {
        display: flex;
        flex-direction: column;
        width: 94%;
        margin: 0 auto;
    }

    .baoming-zaixian {
        text-align: center;
        width: 94%;
        margin: 0 auto;
        height: 80rpx;
        margin-top: 3%;
        font-weight: 700;
        border-bottom: solid 1px #F1F1F2;
    }

    .baoming-xinxi {
        display: flex;
        flex-direction: column;
        width: 94%;
        height: 550rpx;
        margin: 0 auto;
    }

    .xinxi-name,
    .xinxi-phone,
    .xinxi-fenzu,
    .xinxi-miaoshu {
        display: flex;
        flex-direction: row;
        margin-top: 4%;
    }

    .xinxi-name p:nth-child(1),
    .xinxi-phone p:nth-child(1),
    .xinxi-fenzu p:nth-child(1),
    .xinxi-miaoshu p:nth-child(1) {
        width: 30%;
        font-size: 16px;
        color: #9BA7C1;
        line-height: 80rpx;
    }

    .xinxi-name p:nth-child(2),
    .xinxi-phone p:nth-child(2),
    .xinxi-fenzu p:nth-child(2),
    .xinxi-miaoshu p:nth-child(2) {
        width: 60%;
        height: 60rpx;
        font-size: 16px;
        background-color: #FAFBFD;
        /* text-indent: 20rpx; */
        padding-left: 20rpx;
        border: solid 1px #F1F1F2;
        padding-top: 2%;
    }

    .xinxi-miaoshu p:nth-child(2) {
        width: 60%;
        height: 150rpx;
    }

    .xinxi-miaoshu p textarea {
        width: 90%;
        height: 150rpx;
    }

    /* .baoming-photo {
        display: flex;
        flex-direction: column;
        width: 94%;
        height: 200rpx;
        margin: 0 auto;
    }

    .baoming-photo p:nth-child(1) {
        font-size: 15px;
        height: 40rpx;
        color: #B7BABB;
        margin-bottom: 3%;
    }

    .baoming-photo p:nth-child(2) {
        width: 60px;
        height: 60px;
        text-align: center;
        line-height: 60px;
        font-size: 30px;
        color: #B7BABB;
        border: solid 1px #B7BABB;
    }

    .baoming-tijiao {
        display: flex;
        position: fixed;
        bottom: 0;
        flex-direction: row;
        width: 94%;
        height: 80rpx;
        margin: 0 auto;
    }

    .baoming-tijiao button {
        width: 100%;
        height: 80rpx;
        border-radius: 0;
        background-color: #1AAD19;
        color: white;
        text-align: center;
        line-height: 80rpx;
        font-size: 15px;
    } */
    .th-icon-cancel {
        position: absolute;
        background-color: #fff;
        border-radius: 50%;
        right: -14rpx;
        top: -14rpx;
    }

    .weui-uploader__input-box {
        margin-right: 0;
        float: left;

        border: 1rpx dashed gainsboro;
        text-align: center;
        line-height: 150rpx;
    }

    .weui-uploader__bd {
        margin-bottom: 0;
    }

    .posi-rela {
        position: relative;
        overflow: visible;
        width: 150rpx;
        height: 150rpx;
        margin-bottom: 15rpx;
        margin-left: 20rpx;
        float: left;
    }

    .weui-uploader__file:nth-child(4n) {
        margin-right: 0;
    }

    .th-backwhite {
        width: 100%;
        height: 100%;
        /* padding: 20rpx 30rpx; */
        box-sizing: border-box;
        background-color: #fff;
        border-bottom: 4rpx solid #f5f5f5;
        display: flex;

    }

    /* 绿色贯穿按钮 */
    .th-submit-btn {
        width: 690rpx;
        height: 90rpx;
        line-height: 90rpx;
        background-color: #18c136;
        margin: 50rpx auto;
        color: #fff;
        font-size: 34rpx;
        text-align: center;
        border-radius: 6rpx;
    }
</style>