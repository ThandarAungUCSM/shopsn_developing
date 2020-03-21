<template>
    <div>
        <div v-title data-title="抢购">抢购</div>
        <home-header :userName="title"></home-header>
        <!-- <mt-swipe :auto="4000">
            <mt-swipe-item v-for="item in imgs" :key="item.id">
                <img v-lazy="item">
            </mt-swipe-item>
        </mt-swipe> -->
        <ul class="content">
            <li v-for="(item,index) in panicList" :key="index">
                <img :src="URL + item.pic_url" alt="">
                <div class="listChild">
                    <p class="title">{{item.title}}</p>
                    <div>
                        <div>
                            <h4 v-if="(item.start_time*1000) <= currentTimeStamp">抢购中</h4>
                            <h4 v-else class="redTxt">{{formatTime(item.start_time)}}开始抢购</h4>
                            <h5>￥{{item.panic_price}}</h5>
                            <h6>￥{{item.price_market}}</h6>
                        </div>
                        <div>
                            <button v-if="(item.start_time*1000) <= currentTimeStamp"  @click="chase(item.goods_id)">马上抢购</button>
                            <p><span>{{item.already_num}}</span> 人已购买</p>
                        </div>
                    </div>
                </div>
            </li>
        </ul>
    </div>
</template>
<script>
    import HomeHeader from '@/components/home/children/header.vue'; //头部内容
    import qs from 'qs';

    export default {
        data() {
            return {
                notice: "提醒我",
                title: '',
                shopImg: require("@/assets/con23.jpg"),
                panicList: [],
                currentTimeStamp:''
            }
        },
        created() {
            this.getData();
        },
        mounted(){
            var now = new Date();
             this.currentTimeStamp = now.getTime();
        },
        methods: {
            getData() {
                this.axios.post(this.$httpConfig.getPanicGoods).then((res) => {
                    if (res.data.status == 1) {
                        this.panicList = res.data.data.data;
                    }
                }).catch((err) => {
                    console.log(err);
                });
            },
            // 格式化日期，如月、日、时、分、秒保证为2位数
            formatNumber(n) {
                n = n.toString()
                return n[1] ? n : '0' + n;
            },
            // 参数number为毫秒时间戳，format为需要转换成的日期格式
            formatTime(number) {
                let time = new Date(number*1000)
                var month = this.formatNumber(time.getMonth() + 1);
                var date = this.formatNumber(time.getDate());
                var h = this.formatNumber(time.getHours());
                var m = this.formatNumber(time.getMinutes());
                var format = month + "." + date  + ' ' + h + ":" + m
                return format;
            },
            chase(id) {
                this.$router.push({
                    name:'auctionList',
                    params:{
                        goods_id:id
                    }
                })
            },
            notices() {
                this.notice = "已提醒"
            }
        },
        components: {
            HomeHeader
        }
    }
</script>
<style lang="less" scoped>
    .mint-swipe {
        width: 100%;
        height: 4.28rem;

        img {
            width: 100%;
            height: 100%;
        }

        .mint-swipe-indicators {
            .mint-swipe-indicator {
                width: 20px;
                height: 20px;
            }
        }
    }

    .content {
        width: 100%;

        li {
            padding: .1rem .3rem;
            height: 2.55rem;
            background-color: #fff;
            display: flex;
            border-bottom: .01rem solid #B2B2B2;
            align-items: center;

            img {
                width: 2.2rem;
                height: 2.2rem;
            }

            .listChild {
                flex: 1;
                padding-left: .3rem;

                .title {
                    font-size: .28rem;
                    line-height: .4rem;
                    text-overflow: -o-ellipsis-lastline;
                    overflow: hidden;
                    text-overflow: ellipsis;
                    display: -webkit-box;
                    -webkit-line-clamp: 2;
                    line-clamp: 2;
                    -webkit-box-orient: vertical;
                }

                div {
                    display: flex;
                    justify-content: space-between;

                    div {
                        display: flex;
                        flex-direction: column;
                        justify-content: center;
                        align-items: center;

                        h4 {
                            width: 100%;
                            font-size: .28rem;
                            color: #d02629;
                        }

                        h5 {
                            width: 100%;
                            font-size: .32rem;
                            color: #F9282B;
                        }

                        h6 {
                            width: 100%;
                            font-size: .24rem;
                            color: #969696;
                            text-decoration: line-through;
                        }

                        button {
                            width: 1.7rem;
                            height: .65rem;
                            border: 0;
                            margin-bottom: .1rem;
                            background-color: #d02629;
                            color: #fff;
                            border-radius: .04rem;
                        }

                        p {
                            width: 100%;
                            text-align: right;
                            font-size: .24rem;
                        }

                        span {
                            color: #EE2329;
                        }

                        .redTxt {
                            color: #F1302B;
                        }

                        .greenBg {
                            font-size: 0.3rem;
                            background-color: #72BE52;
                        }
                    }
                }
            }
        }
    }
</style>
