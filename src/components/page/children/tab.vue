<template>
    <div class="pr-tab-wrap">
        <div v-title data-title="商品详情">商品详情</div>
        <pr-header :text="title" :number="number"></pr-header>
        <mt-navbar v-model="selected">
            <mt-tab-item id="1">图文详情</mt-tab-item>
            <mt-tab-item id="2">商品属性</mt-tab-item>
            <mt-tab-item id="3">商品评论</mt-tab-item>
             <mt-tab-item id="4">商品咨询</mt-tab-item>
        </mt-navbar>
        <mt-tab-container v-model="selected">
            <mt-tab-container-item id="1">
               <div v-html="imgText" id="pic_article"></div>
            </mt-tab-container-item>
            <mt-tab-container-item id="2">
                <ul class="list-wrap">
                    <li class="clearfix">
                      	<p class="pTitle">商品名称：</p>
                      	<span>{{attrData.title}}</span>
                    </li>
                    <li class="clearfix" v-for="item in attrData.attr" :key="item.id">
                      	<p class="pTitle">{{item.attribute_id}}：</p>
                      	<span>{{item.attr_value}}</span>
                    </li>
                </ul>
            </mt-tab-container-item>
            <mt-tab-container-item id="3">
                <div class="comment-wrap">
                    <ul>
<!--                        <li @click="conActiveClick(index)" v-for="(item,index) in conName" :key="index" :class="{conActive: index== comIndex}">-->
<!--                            <span>{{item}}</span>-->
<!--                            <p v-if="index ==0">{{commenData.allcount}}</p>-->
<!--                            <p v-if="index ==1">{{commenData.nice}}</p>-->
<!--                            <p v-if="index ==2">{{commenData.height}}</p>-->
<!--                            <p v-if="index ==3">{{commenData.bad}}</p>-->
<!--                            <p v-if="index ==4">{{commenData.image}}</p>-->
<!--                        </li>-->
<!--                        <li @click="conActiveClick(index)" v-for="(item,index) in conName" :key="index" :class="{conActive: index== comIndex}">-->
<!--                            <span>{{item}}</span>-->
<!--                            <p v-if="index ==0">{{commentName.allCount}}</p>-->
<!--                            <p v-if="index ==1">{{commentName.allNice}}</p>-->
<!--                            <p v-if="index ==2">{{commentName.allHeight}}</p>-->
<!--                            <p v-if="index ==3">{{commentName.allBad}}</p>-->
<!--                            <p v-if="index ==4">{{commentName.allImg}}</p>-->
<!--                        </li>-->

                        <li :class="comIndex == 0? 'conActive': ''" @click="comContent(0)">
                            <span>全部评价</span>
                            <p>{{ allCount || 0 }}</p>
                        </li>
                        <li :class="comIndex == 1? 'conActive': ''" @click="comContent(1)">
                            <span>好评</span>
                            <p>{{ allNice || 0 }}</p>
                        </li>
                        <li :class="comIndex == 2? 'conActive': ''" @click="comContent(2)">
                            <span>中评</span>
                            <p>{{ allHeight || 0 }}</p>
                        </li>
                        <li :class="comIndex == 3 ? 'conActive': ''" @click="comContent(3)">
                            <span>差评</span>
                            <p>{{ allBad || 0 }}</p>
                        </li>
                        <li :class="comIndex == 4? 'conActive': ''" @click="comContent(4)">
                            <span>有图</span>
                            <p>{{ allImg || 0 }}</p>
                        </li>

                    </ul>
                    <div v-if="dataItem == null" class="center">暂无评论</div>
                    <div v-else class="list-wrap" v-for="item in dataItem" :key="item.id + 'i'">
                        <div class="list-hd clearfix">
                            <div class="pull-left fl">
                                <img src="../../../assets/user-d.jpg">
                                <span >{{item.nick_name}}</span>
                            </div>
                            <div class="pull-right fr">
                                <img v-for = 'n in parseInt(item.score)' :key = 'n' :src="imgYes">
                                <img v-for = 'n in 5-parseInt(item.score)' :key ="n + 'i'" :src="imgNo">
                            </div>
                        </div>
                        <div class="list-com">{{item.content}}</div>
                        <div class="list-img-main clearfix" v-for="items in item.path" :key="items.id + 'j'">
<!--                            <img :class="{'active':isChoose}"  @click="imgScc" :src="URL + items.path" class="fl" v-for="items in item.path" :key="items.id">-->
                            <img :class="{ 'active': tempImgValue == items && isChoose  }"  @click="imgScc(items)" :src="URL + items.path" class="fl" >
                            <!--                            'active':(page === currentPage)}-->
                        </div>
<!--                        <div class="list-purTimer">{{item.goods_apace[0].speName}}&nbsp;&nbsp;&nbsp;{{item.goods_apace[0].item }} &nbsp;&nbsp;&nbsp;{{Number(item.create_time)|formatDate}}</div>-->
                        <div class="list-purTimer">{{Number(item.create_time)|formatDate}}</div>
                    </div>
                </div>
            </mt-tab-container-item>
            <mt-tab-container-item id="4">
               <ul class="product">
                   <li v-for ="item in product" :key="item.id">
                       <div class="use_area_head">
                        <em>用户g***8的提问：</em>
                        <span class="fr g_ask_time">{{formatTime(item.create_time)}}</span>
                       </div>
                       <p class="pro text1-hidden"><span class="g_icon">问</span> {{item.content}}</p>
                       <p class="text1-hidden" v-if="item.answer"><span class="g_icon">答</span>{{item.answer}}</p>
                       <p class="text1-hidden color_grey" v-else><span class="g_icon">答</span> 暂无回答</p>
                       <div class="g_look_more" v-if="item.answer" @click="lookMore(item.id)">查看全部1个回答</div>
                   </li>
               </ul>
               <div class="btm">
                   <input type="text" v-model.trim="problem">
                   <h6 @click="proSubmit">提问</h6>
               </div>
            </mt-tab-container-item>
        </mt-tab-container>
        <div class="load" v-show="load" @touchmove.prevent><mt-spinner type="triple-bounce" color="rgb(38, 162, 255)"></mt-spinner></div>

        <div class="load-wrap" v-show="load_wrap" @touchmove.prevent><mt-spinner type="triple-bounce" color="rgb(38, 162, 255)"></mt-spinner></div>
        <Shopsn></Shopsn>
    </div>
</template>
<script>
    import { Toast } from 'mint-ui';
    import QS from 'qs';
    import PrHeader from './header.vue';
    import Shopsn from '@/components/page/Shopsn.vue';
    export default {
        name : 'tab',
        data(){
            return {
                imgNo:require('@/assets/images/xinxin.png'),
                imgYes:require('@/assets/images/xu_xinxin.png'),
                selected:"1",
                title:'商品详情',
                number:'0',
                data:'',
                commenData:'',
                dataItem:'',
                scrollWatch:true,
                page:1,
                comIndex :0,
                conName:['全部评价', '好评', '中评', '差评', "有图"],
                comId:1,
                product:'',
                problem:'',
                load:false,
                load_wrap:true,
                attrData:'',
                imgText:'',
                text:'',
                sonState:false,
                specData:'',
                isChoose:false,
                commentName: [],
                allCount: 0,
                allNice: 0,
                allHeight: 0,
                allBad: 0,
                allImg: 0,
                tempImgValue: ''
            }
        }, 
        methods:{
            lookMore(id){
                this.$router.push({
                    name:'answerDetails',
                    params:{
                        id:1
                    }
                })
            },
            formatTime(t){
                var time = new Date(Number(t) * 1000);
                var Y = time.getFullYear();
                var m = time.getMonth() + 1;
                var d = time.getDate();
                if (m < 10) {
                    m = '0' + m;
                }
                if (d < 10) {
                    d = '0' + d;
                }
                return (Y + "-" + m + "-" + d);
            },
            // imgScc(){
            //     this.isChoose = !this.isChoose
            // },
            imgScc(value){
                this.tempImgValue = value
                this.isChoose = !this.isChoose
            },
            // 提交咨询
            proSubmit(){
                if(!this.problem){
                     Toast({
                        message: '请输入咨询内容',
                        position: 'bottom',
                        duration: 1000
                    });
                    return;
                }
                this.axios.post(this.$httpConfig.userCommitProblem,QS.stringify({
                    goods_id:this.$route.params.id,
                    content:this.problem
                })).then((res) => {
                     Toast({
                        message:res.data.message,
                        position: 'bottom',
                        duration: 1000
                    });
                    if(res.data.status == 1){
                        this.problem=''
                    }
                }).catch((err) => {
                    console.log(err);
                });
            },
             // 图文详情请求
            imgTextAjax(){
//              this.load = true;
                this.axios.post(this.$httpConfig.getGoodsDetail,
                   QS.stringify({
                        goods_id:this.$route.params.id,
                    })
                ).then((res) => {
                    this.load = false;
                    if(res.data.status == 1){
                        this.imgText = this.intoHtml(res.data.data)
                    }
                }).catch((err) => {
                    console.log(err);
                });
            },
            // 咨询列表
            productAjax(){
                this.load = true;
                this.axios({
                    url:this.$httpConfig.goodsProblems,
                    method:'get',
                    params:{
                        goods_id:this.$route.params.id,
                        p:this.page
                    }
                }).then((res) => {
                    this.load = false;
                    if(res.data.status == 1){
                        this.product = res.data.data.records;
                    }
                }).catch((err) => {
                    console.log(err);
                });
            },
            // 规格参数getGoodsAttr
            GoodsAttr(){
//              this.load = true;
                this.axios.post(this.$httpConfig.getGoodInfo,
                	QS.stringify({
                		 id:this.$route.params.id,
                	})
                ).then((res) => {
                    this.load = false;
                    if(res.data.status == 1){
                        this.attrData = res.data.data;
//                      this.specData = res.data.data.spec;
                    }
                }).catch((err) => {
                    console.log(err);
                });
            },
            // tab切换
            conActiveClick(index){
                if(index == this.comIndex)return
                this.comIndex = index
                switch(this.comIndex){
                     case 0:
                        this.comId = 4;
                        this.load = true;
                        break;
                    case 1:
                        this.comId = 3;
                        this.load = true;
                        break;
                    case 2:
                        this.comId = 2;
                        break;
                    case 3:
                        this.comId = 1;
                        break;
                    case 4:
                        this.comId = 5;
                        break;
                    }
                    this.comContent(0)
            },
            //评论
            comContent(type){
                this.comIndex = type;
                this.load = true;
                 this.axios({
                    url:this.$httpConfig.getAllCommentContent,
                    method:'get',
                    params:{
                        goods_id:this.$route.params.id,
                        type: type,
                        status:4,
                        page:this.page
                    }
                }).then((res) => {
                    this.load = false
                    if(res.data.status==1){
                    	 this.dataItem= res.data.data.data.list;
                        // this.commentName=res.data.data.data;
                        this.allCount = res.data.data.data.allCount;
                        this.allNice = res.data.data.data.allNice;
                        this.allHeight = res.data.data.data.allHeight;
                        this.allBad = res.data.data.data.allBad;
                        this.allImg = res.data.data.data.allImg;
                    }
                   
                }).catch((err) => {
                    console.log(err);
                });
            },
            // 评价数量
            commentAjax(){
                this.axios({
                    url:this.$httpConfig.getAllCommentsCount,
                    method:'get',
                    params:{
                        goods_id:this.$route.params.id,
                        page:1
                    }
                }).then((res) => {
                    this.load = false
                    this.commenData = res.data.data;

                }).catch((err) => {
                    console.log(err);
                });
            },
            intoHtml(str) {
                var arrEntities={'lt':'<','gt':'>','nbsp':' ','amp':'&','quot':'"'};
                return str.replace(/&(lt|gt|nbsp|amp|quot);/ig,function(all,t){return arrEntities[t];});
            }
        },
        watch: {
            selected:function(){
                switch(this.selected){
                    case '1':
                        this.imgTextAjax()
                        break;
                    case '2':
                        this.GoodsAttr();
                        break;
                    case '3':
                        this.commentAjax()
                        this.comContent(0)
                        break;
                    case '4':
                        this.productAjax()
                        break;
                    }
            }
        },
        mounted() {
            this.load_wrap =false;
            
            document.body.scrollTop = 0;
            this.imgTextAjax()
        },
        destroyed(){
            this.scrollWatch = false;
        },
        components:{
            PrHeader,
            Shopsn
        }
    }
</script>
<style lang="less">
    .pr-tab-wrap{
        background:#f1f1f1;
        .list-main{
            padding-top:.1rem;
        }
        #pic_article  img {
            width: 100%;
        }
        .mint-tab-item{
            .mint-tab-item-label{
                color:#333;
                font-size:.3rem;
                line-height:.75rem;
            }
        }
        .mint-tab-item.is-selected{
            .mint-tab-item-label{
                color:#333;
            }
        }
        .mint-navbar{
            height:.75rem;
            background:#fff;
            line-height:.75rem;
            margin: 0 0 .2rem 0;
            .mint-tab-item{
                height:100%;
                padding:0;
                border-color:#d02629;
                line-height:.75rem;
            }
        }
        .center{
            text-align: center;
            height: 3rem;
            line-height: 10rem;
            font-size: .4rem;
        }
        .list-wrap{
            padding-top:.2rem;
            li{
                padding:.25rem .2rem;
                border-bottom:1px solid #e7e7e7;
                background:#fff;
                .title{
                    width:2.28rem;
                    font-size:.28rem;
                    color:#696969;
                }
                .con{
                    width:4.8rem;
                    font-size:.26rem;
                    color:#333;
                    line-height:.36rem;
                    text-align: right;
                }
                .pTitle{
                	width: 2rem;
                	/*height: 1rem;*/
                	float: left;
                	font-size: .25rem;
                	color: rgb(123,123,123);
                }
                span{
                	display: block;
                	width: 4.5rem;
                	float: left;
                	line-height: 150%;
                	font-size: .25rem;
                	color: #333333;
                }
            }
            .list-com{
                height:1rem;
                font-size:.24rem;
                color:#333;
                display: -webkit-box;
                overflow: hidden;
                white-space: normal!important;
                text-overflow: ellipsis;
                word-wrap: break-word;
                -webkit-line-clamp: 3;
                -webkit-box-orient: vertical;
            }
        }
        .mint-tab-container {
            overflow: visible;
            position: relative;
        }
        .comment-wrap{
            ul{
                height: 105/100rem;
                display: flex;
                li{
                    width: 20%;
                    height: 100/100rem;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    flex-direction: column;
                    span{
                        color:#B9B9B9;
                        font-size: 32/100rem;
                        margin-bottom:10/100rem;
                    }
                    p{
                        font-size: 28/100rem;
                    }
                }
                .conActive{
                    border-bottom: 5/100rem solid #d02629;
                }
            }
            h2{
                padding:0 .2rem;
                height:.88rem;
                font-size:.32rem;
                line-height:.88rem;
                background:none;
                color:#333;
                span{
                    font-size:.32rem;
                    color:#f9781e;
                }
            }
            .list-wrap{
                width:7.1rem;
                padding:0 .2rem;
                height:3.5rem;
                background:#fff;
                margin-bottom:.1rem;
                .list-hd{
                    padding:.2rem 0;
                    line-height:.32rem;
                    height:.3rem;
                    .pull-left{
                        padding-left:.46rem;
                        font-size:.2rem;
                        color:#b0b0b0;
                        position:relative;
                        img{
                            width:.32rem;
                            height:.32rem;
                            position:absolute;
                            left:0;
                            top:0;
                        }
                    }
                    .pull-right{
                        color:#999;
                        font-size:.26rem;
                        img{
                            width: 30/100rem
                        }
                    }
                }
                .list-img-main{
                    height:1rem;
                    img{
                        width:1rem;
                        height:1rem;
                        margin-right:.2rem;
                        transform: scale(1);            /*图片原始大小1倍*/
                        transition: all ease 0.5s;      /*图片放大所用时间*/
                    }
                    img.active {
                        transform: scale(5);          /*图片需要放大3倍*/
                        position: absolute;           /*是相对于前面的容器定位的，此处要放大的图片，不能使用position：relative；以及float，否则会导致z-index无效*/
                        z-index: 100;
                        margin-left: 3rem;
                    }
                }
                .list-purTimer{
                    padding-top:.2rem;
                    font-size:.2rem;
                    color:#999;
                }
            }
        }
        .list-img-wrap{
            background:#fff;
            margin-top:.2rem;
            img{
                width:100%;
            }
        }
    }
</style>
 <style lang="less" scoped>
     .product{
        margin-top:20/100rem;
        margin-bottom: 1.2rem;
        li{
            background-color: #fff;
            padding:10/100rem 20/100rem;
            margin-bottom: .2rem;
            .use_area_head{
                margin-bottom: .18rem;
                height: .7rem;
                line-height: .7rem;
                color: #666;
                font-size: .24rem;
                em{
                    font-style: normal;
                }
                .g_ask_time{
                    color:#666;
                    font-size:.24rem;
                }
            }
            .pro{
                font-weight: 700;
                margin-bottom:.05rem;
                span{
                    background-color: #CA9B27;
                    
                }
            }
            .color_grey{
                color: #999;
            }
            p{
                color: #333;
                font-size: .32rem;
                margin-bottom:.1rem;
                font-size: 32/100rem;
                line-height: 65/100rem;
            }
            .g_icon{
                font-size: 28/100rem;
                padding: 6/100rem;
                border-radius: 5/100rem;
                color:#fff;
                background-color: #97CD93;
            }
            .g_look_more{
                font-size:.24rem;
                color:#3985ff;
                text-align:right;
            }
            .g_look_more::after {
                content: "";
                display: block;
                width: 8px;
                height: 8px;
                border-top: 1px solid #3985ff;
                border-left: 1px solid #3985ff;
                -webkit-transform-origin: 50%;
                transform-origin: 50%;
                -webkit-transform: rotate(135deg);
                transform: rotate(135deg);
                margin: -2px 0 0 5px;
                display: inline-block;
                vertical-align: middle;
            }
        }
        
    }
    .btm{
        width:100%;
        height:100/100rem;
        background-color: #fff;
        display: flex;
        padding:15/100rem;
        box-sizing: border-box;
        position: fixed;
        bottom:0;
        input{
            border-radius: 15/100rem;
            width: 615/100rem;
            height: 70/100rem;
            background-color:#F1F1F1;
            order: 0;
            outline: none; 
            margin-right:20/100rem;
            padding:20/100rem;
            font-size: 24/100rem;
        }
        h6{
            font-size: 36/100rem;
            line-height: 60/100rem;
            color: #D77649;
        }
    }
</style>
