<template>
  <div class="product">
    <div v-title data-title="商品详情">商品详情</div>
    <pr-header :text="title"></pr-header>
    <mt-swipe :auto="0">
      <mt-swipe-item v-for="item in images" :key="item.id">
        <img :src="URL + item.pic_url" />
      </mt-swipe-item>
    </mt-swipe>
    <div class="describe" v-if="$store.state.commodity_data">
      <div>


        <div v-if="opt">
          <p class="fn" v-if="detail">{{$store.state.commodity_data.goods.title}}</p>
          <p class="fn" v-else>{{$store.state.commodity_data.goods.title}}</p>
        </div>
        <div v-if="!opt">
          <p class="fn" v-if="detail">{{$store.state.commodity_data.goods.title}}</p>
          <!-- <p class="fn" v-else>{{$store.state.commodity_data.goods.title}}{{option}}</p> -->
          <p class="fn" v-else>{{$store.state.commodity_data.goods.title}}</p>
        </div>

      </div>
      <!--      <p class="fn" v-if="!opt">{{$store.state.commodity_data.goods.title}}{{option}}</p>-->
      <!--      <p class="fn" v-if="opt">{{$store.state.commodity_data.goods.title}}</p>-->

      <p class="price" v-if="$store.state.commodity_data.goods.p_id==0">
        <span style="color:red;font-size:20px">
          ￥
          {{productData.price_member}}
        </span>
        <span class="new">
          原价 ：￥
          <s>{{productData.price_market}}</s>
        </span>
        <!--        <span class="report" @click="Report">举报</span>-->
        <span class="report" @click="handleNotice">举报</span>
      </p>
      <p class="price" v-else>
        <span style="color:red;font-size:20px">
          ￥
          {{$store.state.commodity_data.goods.price_member}}
        </span>
        <span class="new">
          原价 ：￥
          <s>{{$store.state.commodity_data.goods.price_market}}</s>
        </span>
        <!--        <span class="report" @click="Report">举报</span>-->
        <span class="report" @click="handleNotice">举报</span>
      </p>
    </div>

    <!--promotion-->
    <div v-if="detailId == detialSubId">
    <div v-if="promotion.length > 0" @click="showPromotion">
      <div class="promotion">
        <p class="title">促销</p>
          <div class="body" v-for="(item,index) in promotion" :key="index">
            <p class="text" v-if="index==0">
              <span class="name">{{item.name}}</span>
              <span class="full">(满 {{item.full}} 元， 数量 {{item.give_count}} 赠完即止)</span>
            </p>
            <p class="text" v-if="index==1">
              <span class="name">{{item.name}}</span>
              <span class="full">满 {{item.full}} 减 {{item.expression}}</span>
            </p>
          </div>
        </div>
        <span class="btn-right"></span>
      </div>
      <div class="fenge"></div>
    </div>
    <div v-else>
    </div>

    <transition  name="fade">
      <div class="promotion_box" v-if="pro">
        <div class="promotion_main">
          <div class="promotion_header">
            <p class="promotion_text">优惠</p>
            <span class="promotion_cross" @click="showPromotion">×</span>
          </div>
          <div class="box_promotion">
          </div>
        </div>
      </div>
    </transition>

    <!-- 已选 -->
    <div class="selected" @click="theSon">
      <span class="title">规格</span>
      <div v-if="opt">
        <span style="float:right; padding-right: .2rem; font-size: .26rem; margin-top: -.8rem;" v-if="opt" @click="theSon">{{detail}}</span>
        <span style="float:right; padding-right: .2rem; font-size: .26rem; margin-top: -.8rem;" v-else>{{detail}}</span>
      </div>
      <div v-if="!opt">
        <span style="float:right; padding-right: .2rem; font-size: .26rem; margin-top: -.8rem;" v-if="detail !== ''">{{detail}}</span>
        <span style="float:right; padding-right: .2rem; font-size: .26rem; margin-top: -.8rem;" v-else>{{option}}</span>
      </div>



      <span
              v-if="item"
              v-for="(item, index) in $store.state.guigeidname"
              :key="index"
              class="number"
      >{{$store.state.guigeName[index]}}&nbsp;{{item}}</span>
      <span class="btn-right"></span>
    </div>
    <div class="fenge"></div>

    <div class="for-discount" @click="showCoupon">
      <span class="discount_text">优惠</span>
      <span
              class="coupon_img"
              v-for="(item,index) in couponList.slice(0,3)"
              :key="index.id">
         {{item.name}}
     </span>
      <!--      <span-->
      <!--              class="coupon_img"-->
      <!--              >超级用户 VIP-->

      <!--      </span>-->
      <!--      <span-->
      <!--              class="coupon_message"-->
      <!--              v-for="item in data"-->
      <!--      >{{item.message}}-->

      <!--            </span>-->
      <span class="btn-right"></span>
      <!-- <div class="for-security">
          <button>产地证明</button>
          <span class="btn-right"></span>
      </div> -->
      <!-- <div class="for-security"> -->
      <!-- <button>朔源码</button> -->
      <!-- <span>{{newgood.source_code}}</span> -->
      <!-- </div> -->
    </div>
    <div class="fenge"></div>

    <transition  name="fade">
      <div class="coupon_box" v-if="sta">
        <div class="box_main">
          <div class="box_header">
            <p class="box_text">领券</p>
            <span class="box_cross" @click="showCoupon">×</span>
          </div>
          <div class="box_box">
            <div class="box_body" v-for="(item,index) in couponList" :key="index.id" @click="couponReceive(index)">
              <div class="body_left">
                <span class="left_money">￥{{item.money}}</span>
                <span class="left_name">{{item.name}}</span>
                <p class="condition">满{{item.condition}}元可以使用</p>
                <p class="left_time">有效期 {{new Date(item.use_start_time * 1000).getFullYear()+'.'+(new Date(Number(item.use_start_time * 1000)).getMonth()+1)+'.'+new Date(Number(item.use_start_time * 1000)).getDate()}} - {{new Date(item.use_end_time * 1000).getFullYear()+'.'+(new Date(Number(item.use_end_time * 1000)).getMonth()+1)+'.'+new Date(Number(item.use_end_time * 1000)).getDate()}}</p>
                <span class="body_circle"></span>
                <span class="body_circle1"></span>
              </div>
              <div class="body_right">立即领取</div>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <!-- 店铺 -->
    <shop-infor :shopData="shopData" :data="$store.state.commodity_data"></shop-infor>
    <!-- 搭配套餐推荐 -->
    <pr-list
            v-show="$store.state.matchGood"
            :conItem="conItemRe"
            :storeId="shopData.id"
            :val="1"
            :data="$store.state.matchGood"
    ></pr-list>
    <!-- 猜你喜欢 -->
    <pr-list
            v-if="$store.state.dataLeave.length != 0 || $store.state.dataLeave.length"
            :conItem="conItem"
            :val="2"
            :data="$store.state.dataLeave"
    ></pr-list>

    <div class="prompt" @click="toTab">点击查看更多商品信息</div>
    <detail-option
            @recommend="matchGood($store.state.commodity_data.goods.id)"
            v-if="guige"
            :data="$store.state.commodity_data"
            :productData="productData"
            @sendValue="sendValue"
    ></detail-option>
    <Shopsn></Shopsn>
    <foot-btn
            :storeid="store__id"
            :state="sonState"
            :Number="$store.state.commodity_val"
            :data="$store.state.commodity_data"
            :productData = "productData"
            @reduce="reduce"
            @plus="plus"
            :money="this.$route.params.money"
    ></foot-btn>
    <div class="load-wrap" v-show="load_wrap" @touchmove.prevent>
      <mt-spinner type="triple-bounce" color="rgb(38, 162, 255)"></mt-spinner>
    </div>
  </div>
</template>
<script>
  import PrHeader from "@/components/page/children/shop_header.vue"; // 头部
  import prList from "@/components/page/children/list.vue"; //猜你喜欢和搭配套餐
  import FootBtn from "@/components/page/children/footBtn.vue"; // 底部按钮
  import PageOption from "@/components/page/children/pageOption.vue";
  import shopInfor from "@/components/page/children/shopInfor.vue"; // 店铺信息
  import detailOption from "@/components/page/children/detailOptions.vue"; // 立即购买 || 加入购物车 弹框
  import Shopsn from "@/components/page/Shopsn.vue";
  import qs from "qs";
  import { Popup } from "mint-ui";
  import { Toast } from 'mint-ui';
  import CountDown from "vue2-countdown";
  export default {
    name: "product",
    data() {
      return {
        pro:false,
        sta:false,
        couponList:[],
        detail: '',
        detailId: null,
        detialSubId: null,
        option: '',
        opt:false,
        promotion: '',
        promotion_id: '',
        route_id: '',
        //是否显示状态条
        showIndicators: true,
        //初始轮播切换索引
        defaultIndex: 0,
        //轮播是否循环播放
        continuous: true,
        number: 1,
        conItem: {
          title: "猜你喜欢"
        },
        conItemRe: {
          title: "搭配套餐推荐"
        },
        scrollWatch: true,
        topStatus: "",
        loadTop: {},
        sonState: false,
        data: "",
        title: "商品详情",
        dataLeave: "",
        load_wrap: true,
        link_id: this.$route.params.id,
        guige: false,
        shopData: "",
        popupVisible: false,
        endday: "",
        end: "",
        brand: "",
        finish: false,
        images: [],
        page: 1,
        p_id: 0,
        store__id: 0,
        specification: '',
        specification_id: '',
        specific: '',
        specId: '',
        IId: '',
        specific_id: '',
        productData: '',
        detailAllData: ''
      };
    },

    created() {
      this.$store.state.const_join = false;
      this.$store.state.catr_number = 0;
      this.$store.state.goods_id = "";
      this.$store.state.matchGood = "";

      this.axios
              .post(
                      this.$httpConfig.goodSpecsByGoodsChildren,
                      qs.stringify({
                        id: this.$route.params.id
                      })
              )
              .then(res => {
                if (res.data.status == 1) {
                  this.specification = res.data.data;
                  this.option = res.data.data.spec.spec_children[0].item;
                  this.IId = res.data.data.spec.spec_children[0].id;
                  for (let i in this.specification.goods) {
                    this.specId = res.data.data.goods[i].key.split("_");
                    if(this.IId == this.specId) {
                      this.specific_id = res.data.data.goods[i].goods_id;

                      this.axios({
                        url: this.$httpConfig.goodInfo,
                        method: "get",
                        params: {
                          id: this.specific_id
                        }
                      })
                              .then(res => {
                                this.productData = res.data.data.goods;
                                this.promotion = res.data.data.promotion;
                              })
                              .catch(err => {
                                console.log(err);
                              });
                      this.axios
                              .post(
                                      this.$httpConfig.matchGood,
                                      qs.stringify({
                                        goods_id: this.specific_id
                                      })
                              )
                              .then(res => {
                                this.$store.state.matchGood = res.data.data;
                              })
                              .catch(err => {
                                console.log(err);
                              });
                    }
                  }
                }
              })
              .catch(err => {
                console.log(err);
              });
    },
    methods: {
      handleNotice() {
        location.replace(window.location.origin +
                "/" +"informOutline?x=" +
                1 +
                "&notice=1" +
                "&store_id=" +
                this.store__id +
                "&goods_id=" +
                this.route_id+'&main_id='+this.p_id)
      },
      countDown() {
        this.finish = true;
      },
      toTab() {
        this.$router.push({
          name: "tab",
          params: {
            id: this.$route.params.id,
            p_id: this.p_id
          }
        });
      },
      theSon() {
        this.$store.state.const_join = true;
        this.opt = !this.opt;

        this.axios({
          url: this.$httpConfig.goodInfo,
          method: "get",
          params: {
            id: this.specific_id
          }
        })
                .then(res => {
                  this.productData = res.data.data.goods
                })
                .catch(err => {
                  console.log(err);
                });
      },
      myLove() {
        this.axios
                .post(this.$httpConfig.guessLove, qs.stringify({ page: this.page }))
                .then(res => {
                  this.$store.state.dataLeave = res.data.data;
                })
                .catch(err => {
                  console.log(err);
                });
      },
      //属性
      // spec() {},
      optionSpec() {

        this.axios
                .post(
                        this.$httpConfig.goodSpecsByGoodsChildren,
                        qs.stringify({
                          id: this.$route.params.id
                        })
                )
                .then(res => {
                  if (res.data.status == 1) {
                    this.specification = res.data.data;
                    this.option = res.data.data.spec.spec_children[0].item;
                    this.IId = res.data.data.spec.spec_children[0].id;
                    for (let i in this.specification.goods) {
                      this.specId = res.data.data.goods[i].key.split("_");
                      if(this.IId == this.specId) {
                        this.specific_id = res.data.data.goods[i].goods_id;

                        this.axios({
                          url: this.$httpConfig.goodInfo,
                          method: "get",
                          params: {
                            id: this.specific_id
                          }
                        })
                                .then(res => {
                                  this.productData = res.data.data.goods;
                                  this.promotion = res.data.data.promotion;
                                })
                                .catch(err => {
                                  console.log(err);
                                });
                      }
                    }
                  }
                })
                .catch(err => {
                  console.log(err);
                });
      },
      optionSpec2() {

        this.axios
                .post(
                        this.$httpConfig.goodSpecsByGoodsChildren,
                        qs.stringify({
                          id: this.$route.params.id
                        })
                )
                .then(res => {
                  if (res.data.status == 1) {
                    this.specification = res.data.data;
                    this.option = this.detailAllData.item
                    this.IId = this.detailAllData.id
                    for (let i in this.specification.goods) {
                      this.specId = res.data.data.goods[i].key.split("_");
                      if(this.IId == this.specId) {
                        this.specific_id = res.data.data.goods[i].goods_id;

                        this.axios({
                          url: this.$httpConfig.goodInfo,
                          method: "get",
                          params: {
                            id: this.specific_id
                          }
                        })
                                .then(res => {
                                  this.productData = res.data.data.goods;
                                  this.promotion = res.data.data.promotion;
                                })
                                .catch(err => {
                                  console.log(err);
                                });
                      }
                    }
                  }
                })
                .catch(err => {
                  console.log(err);
                });
      },
      matchGood(id) {
        this.axios
                .post(
                        this.$httpConfig.matchGood,
                        qs.stringify({
                          goods_id: id
                        })
                )
                .then(res => {
                  this.$store.state.matchGood = res.data.data;
                })
                .catch(err => {
                  console.log(err);
                });
      },
      shopInfo() {
        this.axios({
          url: this.$httpConfig.shopInfo,
          method: "get",
          params: {
            id: this.$store.state.commodity_data.goods.store_id
          }
        })
                .then(res => {
                  this.shopData = res.data.data;
                })
                .catch(err => {
                  console.log(err);
                });
      },
      //商品详情
      ax() {
        this.axios({
          url: this.$httpConfig.goodInfo,
          method: "get",
          params: {
            id: this.$route.params.id
          }
        })
                .then(res => {
                  if (res.data.data.cart_count) {
                    this.$store.state.catr_number = parseInt(res.data.data.cart_count);
                  }
                  this.brand = res.data.data.brand_id;
                  this.images = res.data.data.images;
                  this.brand = res.data.data.brand_id;
                  this.$store.state.commodity_data = res.data.data;
                  this.$store.state.commodity_val = 1;
                  this.matchGood(this.$store.state.commodity_data.goods.id);
                  this.shopInfo();
                  this.guige = true;
                  this.load_wrap = false;
                  this.p_id = res.data.data.goods.p_id;
                  this.store__id = res.data.data.goods.store_id;
                  this.route_id = res.data.data.goods.id;
                  this.promotion = res.data.data.promotion;
                  // console.log(this.promotion)
                  this.promotion_id = res.data.data.promotion.goods_id;
                  // console.log(this.promotion_id)
                })
                .catch(err => {
                  console.log(err);
                });
      },
      CouponSendList() {
        this.axios({
          url: this.$httpConfig.CouponSendList,
          method: "get",
          params: {
            store_id: 2,
          }
        })
                .then(res => {
                  this.couponList = res.data.data.records;
                })
                .catch(err => {
                  console.log(err);
                });
      },
      showPromotion() {
        this.pro = !this.pro;
      },
      showCoupon() {
        this.sta = !this.sta;
      },
      sendValue(detail, id, subid){
        this.detail = detail.item
        this.detailId = id
        this.detialSubId = subid
        this.detailAllData = detail
        this.optionSpec2();
      },
      couponReceive(id) {
        let selectedId = this.couponList[id]
        this.axios({
          url: this.$httpConfig.couponReceiveBehavior,
          method: "get",
          params: {
            id: selectedId.id
          }
        })
                .then(res => {
                  Toast({
                    message: res.data.message,
                    position: "middle",
                    duration: 1000
                  });
                })
                .catch(err => {
                  console.log(err);
                });
      },
      reduce() {
        if (this.number <= 1) return;
        this.number--;
      },
      plus() {
        if (this.number >= this.data.minStock) return;
        this.number++;
      },
      fenxiang() {
        this.popupVisible = !this.popupVisible;
      },
      off() {
        this.popupVisible = false;
      },

    },
    mounted() {
      this.ax();
      this.myLove();
      // this.spec();
      this.CouponSendList();
      this.optionSpec();
      $("html,body").animate(
              {
                scrollTop: "0px"
              },
              100
      );
    },
    destroyed() {
      this.scrollWatch = false;
    },
    components: {
      PrHeader,
      Shopsn,
      prList,
      FootBtn,
      shopInfor,
      PageOption,
      detailOption,
      CountDown
    }
  };
</script>
<style lang="less" scoped>
  .Promotions {
    display: flex;
    padding: 15/100rem 20/100rem;
    h4 {
      color: #a2a2a2;
      font-size: 32/100rem;
      margin-right: 20/100rem;
    }
    .promotionsDiv {
      display: flex;
      padding: 10/100rem;
      align-items: center;
      span {
        padding: 8/100rem;
        color: #e2252b;
        border: 1/100rem solid #e2252b;
        font-size: 24/100rem;
        // width: 100/100rem ;
        height: 25/100rem;
        display: flex;
        justify-content: center;
        align-items: center;
      }
      p {
        margin-left: 20/100rem;
        font-size: 24/100rem;
        color: #656565;
      }
    }
  }

  .fenge {
    height: 20/100rem;
    background-color: #f1f1f1;
  }

  .product {
    background: #fff;
  }
  .for-discount {
    margin: .15rem .2rem;
    height: .42rem;
    padding-top: .06rem;
    /*margin: .2rem;*/
    /*border-bottom: .01rem solid #F1F1F1;*/
    padding-bottom: 0.13rem;
    .discount_text {
      font-size: .26rem;
      color: #777;
      /*padding-top: .1rem;*/
      /*vertical-align: middle;*/
    }
    .coupon_img {
      background: url(../../assets/coupon.png) no-repeat;
      background-size: 100% 100%;
      // width: 2.2rem;
      // height: .44rem;
      font-size: .26rem;
      color: #ffffff;
      text-align: center;
      padding: .08rem .3rem;
      line-height: 1.91;
      margin-left: .2rem;
    }
    .btn-right {
      position: absolute;
      right: 0.3rem;
      /*top: 79%;*/
      margin-top: 0.07rem;
      width: 0.16rem;
      height: 0.3rem;
      background: url(../../assets/btn-right.png) no-repeat;
      background-size: 100% 100%;
    }
    img {
      width: 2rem;
      height:0.5rem;
      padding-left: .2rem;
    }
    .for-security {
      margin: .2rem;
      button {
        background: #FAE9E6;
        color: #DC4703;
        border: none;
        width: 1.3rem;
        height: 0.4rem;
        font-size: .26rem;
        margin: 0.07rem 0 0 .6rem;
        border-radius: .05rem;
      }
      img {
        width: 2.7rem;
        // height: 1rem;
        height:0.6rem;
        margin: 0 0 0 .7rem;
      }
      span{
        font-size: .3rem;
        margin: 0 0 0 2.3rem;
      }
      .btn-right {
        width: 0.22rem;
        height: 0.32rem;
        margin: .15rem 0 0 0;
        background: url(../../assets/btn-right.png) no-repeat;
        background-size: 100% 100%;
      }
    }
  }

  .promotion_box {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1;
    .promotion_main {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      background: #fff;
      border-top-left-radius: .3rem;
      border-top-right-radius: .3rem;
      .promotion_header {
        padding: .4rem .3rem;
        text-align: center;
        border-bottom: 1px solid #f1f1f1;
        .promotion_text  {
          font-size: .34rem;
          color: #333;
        }
        .promotion_cross {
          position: absolute;
          right: 3%;
          top: 0.38rem;
          width: 0.4rem;
          line-height: 0.34rem;
          height: 0.4rem;
          border: 1px solid #bfbfbf;
          text-align: center;
          font-size: 0.4rem;
          color: #bfbfbf;
          border-radius: 100%;
        }
      }
      .box_promotion {
        padding: 0 0 .3rem 0;
        height: 8.85rem;
        overflow-y: scroll;
        position: relative;
      }
    }
  }

  .coupon_box {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1;
    .box_main {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      background: #fff;
      border-top-left-radius: .3rem;
      border-top-right-radius: .3rem;
      .box_header {
        padding: .4rem .3rem;
        text-align: center;
        border-bottom: 1px solid #f1f1f1;
        .box_text  {
          font-size: .34rem;
          color: #333;
        }
        .box_cross {
          position: absolute;
          right: 3%;
          top: 0.38rem;
          width: 0.4rem;
          line-height: 0.34rem;
          height: 0.4rem;
          border: 1px solid #bfbfbf;
          text-align: center;
          font-size: 0.4rem;
          color: #bfbfbf;
          border-radius: 100%;
        }
      }
      .box_box {
        padding: 0 0 .3rem 0;
        height: 8.85rem;
        overflow-y: scroll;
        position: relative;
        .box_body {
          padding: .3rem;
          margin-bottom: 1.5rem;
          color: #dc7676;
          .body_left {
            background: url(../../assets/background1.png) no-repeat;
            background-size: 100% 100%;
            width: 4.57rem;
            height: 1.8rem;
            left: 3%;
            position: absolute;
            border-right: 2px dotted #dc7676;
          }
          .left_money {
            font-size: .42rem;
            margin: .2rem;
            line-height: 1.1rem;
          }
          .left_name {
            font-size: .3rem;
          }
          .condition{
            padding-left: .3rem;
            line-height: 0.55rem;
            margin-top: -0.3rem;
          }
          .left_time {
            font-size: .26rem;
            margin-left: .26rem;
          }
          .body_circle {
            background: url(../../assets/circle.png) no-repeat;
            background-size: 100% 100%;
            position: absolute;
            width: .3rem;
            height: .3rem;
            top: -7%;
            left: 96.2%;
            z-index: 1;
          }
          .body_circle1 {
            background: url(../../assets/circle.png) no-repeat;
            background-size: 100% 100%;
            position: absolute;
            width: .3rem;
            height: .3rem;
            top: 91%;
            right: -2.7%;
            z-index: 1;
          }
          .body_right {
            background: url(../../assets/background2.png) no-repeat;
            background-size: 100% 100%;
            width: 2.5rem;
            height: 1.8rem;
            right: 3%;
            position: absolute;
            font-size: .3rem;
            color: #dc7676;
            text-align: center;
            line-height: 1.8rem;
          }
        }
      }
    }
  }

  .describe {
    padding: 0 0.2rem 0.35rem 0.2rem;
    position: relative;
    background-color: #f1f1f1;
    .fn {
      font-size: 0.36rem;
      color: #333;
      padding-top: 0.3rem;
      line-height: 0.5rem;
      display: -webkit-box;
      overflow: hidden;
      white-space: normal !important;
      text-overflow: ellipsis;
      word-wrap: break-word;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
    }
    .timer {
      font-size: 0.25rem;
      color: #e02828;
      padding: 0.3rem 0;
      .newPos {
        color: #f1302b;
      }
    }
    .price.active {
      .orc {
        line-height: 0.4rem;
        padding-bottom: 0.2rem;
        .icon {
          width: 0.4rem;
          height: 0.4rem;
          background: url(../../assets/integral.png) no-repeat;
          background-size: 100% 100%;
          margin-right: 0.2rem;
        }
      }
      .new {
        padding: 0;
      }
    }
    .price {
      padding-top: 0.2rem;
      .orc {
        color: #e02828;
        font-size: 0.32rem;
        em {
          font-style: normal;
          font-size: 0.48rem;
        }
      }
      .new {
        color: #757575;
        font-size: 0.25rem;
        padding-left: 0.2rem;
        s {
          font-size: 0.24rem;
        }
      }
      .report {
        float: right;
        font-size: .28rem;
        color: red;
      }
    }
    .share {
      position: absolute;
      right: 0.2rem;
      top: 0.3rem;
      text-align: center;
      .icon {
        display: block;
        width: 0.39rem;
        height: 0.42rem;
        background: url(../../assets/share.png) no-repeat;
        background-size: 100% 100%;
      }
      p {
        font-size: 0.24rem;
        color: #555;
        line-height: 0.5rem;
      }
      ul {
        position: absolute;
        top: 45px;
        left: -225%;
        li {
          width: 0.4rem;
          height: 0.4rem;
          float: left;
          margin: 3px;
        }
        img {
          width: 100%;
          height: 100%;
        }
      }
    }
  }

  .promotion {
    padding: 0.1rem 0.5rem 0.1rem 0.2rem;
    color: #777;
    position: relative;
    background: #fff;
    .title {
      font-size: .26rem;
      padding: 0.15rem 0.2rem 0 0;
      position: absolute;
    }
    .body {
      margin-left: .7rem;
      .text {

        .name {
          font-size: .26rem;
          border: 1px solid #d02629;
          border-radius: .06rem;
          color: #d02629;
          line-height: 2.5;
          padding: 0 .04rem;
        }
        .full {
          font-size: .26rem;
          padding-left: .1rem;
        }
      }
    }
    .btn-right {
      position: absolute;
      right: 0.3rem;
      top: 50%;
      margin-top: -0.15rem;
      width: 0.16rem;
      height: 0.3rem;
      background: url(../../assets/btn-right.png) no-repeat;
      background-size: 100% 100%;
    }
  }

  .selected {
    padding: 0 0.5rem 0 0.2rem;
    min-height: 0.8rem;
    line-height: 0.8rem;
    font-size: 0.3rem;
    color: #777;
    position: relative;
    background: #fff;
    border-top: 1px solid #f1f1f1;
    .title {
      font-size: .26rem;
      color: #777;
    }
    .number {
      font-size: 0.28rem;
      color: #333;
      padding-left: 0.55rem;
    }
    .btn-right {
      position: absolute;
      right: 0.3rem;
      top: 50%;
      margin-top: -0.15rem;
      width: 0.16rem;
      height: 0.3rem;
      background: url(../../assets/btn-right.png) no-repeat;
      background-size: 100% 100%;
    }
  }

  .mint-swipe {
    height: 6.22rem;
    .mint-swipe-items-wrap {
      div {
        width: 100%;
        height: 6.22rem;
        img {
          width: 100%;
          height: 100%;
        }
      }
    }
  }

  .prompt {
    width: 100%;
    height: 0.9rem;
    background: #f1f1f1;
    line-height: 0.9rem;
    text-align: center;
    font-size: 0.26rem;
    color: #999;
  }

  #popup {
    width: 7rem;
    padding: 0.5rem 0.5rem 0.2rem;
    background: rgba(0, 0, 0, 0.5);
    ul {
      overflow: hidden;
      border-bottom: 1px solid #dfe3e4;
      background: #fff;
      border-radius: 0.6rem;
      padding: 0.2rem;
      li {
        float: left;
        margin: 0.1rem;
        width: 0.8rem;
        text-align: center;
      }
      img {
        width: 0.8rem;
        height: 0.8rem;
      }
    }
    button {
      display: block;
      width: 100%;
      text-align: center;
      margin-top: 0.2rem;
      background: #fff;
      line-height: 0.8rem;
      border-radius: 0.6rem;
      border: none;
    }
  }
</style>