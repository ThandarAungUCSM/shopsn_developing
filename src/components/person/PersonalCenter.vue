<template>
  <div class="per-center">
    <div v-title data-title="个人中心">个人中心</div>
    <div class="header-wrap">
      <div class="header-hd">
        个人中心
        <router-link to="/myNews">
          <span></span>
        </router-link>
      </div>
      <div class="header-dd-center">
        <!--<div class="setUp"></div>-->
        <div class="icon-wrap" @click="toLink('/seetin')">
          <img v-if="data.user_header" :src="URL+ data.user_header" />
          <img v-else src="../../assets/my_user_pic.png" />
        </div>
        <div class="admin" @click="toLink('/seetin')">
          <span></span>账户管理
        </div>
      </div>
      <div class="status">
        <span class="name">{{data.user_name}}</span>
        <span class="member">{{data.level_name}}</span>
      </div>
    </div>
    <div class="myOrder clearfix">
      <div class="title fl">
        <span class="icon"></span>我的订单
      </div>
      <div class="see fr" @click="toOrder_list">
        <span class="icon-right"></span>查看订单
      </div>
    </div>
    <ul class="nav-link clearfix">
      <li class="fl" v-for="(item,index) in navCon" :key="item.id" @click="toOrder(index)">
        <div class="icon-wrap">
          <img :src="item.imgData" alt />
        </div>
        <p>{{item.text}}</p>
      </li>
    </ul>
    <!-- <div class="myOrder clearfix">
            <div class="title fl"><span class="icon-wallet"></span>我的钱包</div>
            <div class="see fr" @click="toLink('/myWallet')"><span class="icon-right"></span>查看详情</div>
    </div>-->
    <!-- <ul class="account clearfix">
            <li class="fl" @click="toLink('/myWallet')">
                <p class="number"><span class="money">￥</span><span class="min">{{Number($store.state.my_wallet.balance)}}</span></p>
                <p class="balance">账户余额</p>
            </li>
            <li class="fl" @click="toLink('/Integral')">
                <p class="number">{{Number($store.state.my_wallet.integral)}}</p>
                <p class="balance">积分</p>
            </li>
    </ul>-->
    <ul class="more clearfix">
      <li class="fl" v-for="(item,index) in more.icon" :key="item.id" @click="toLink(index)">
        <div class="icon-main">
          <img :src="item" />
        </div>
        <p>{{more.name[index]}}</p>
      </li>
    </ul>
    <!-- <Shopsn></Shopsn> -->
  </div>
</template>
<script>
import qs from "qs";
import Shopsn from "@/components/page/Shopsn.vue";
import "../../config/getUrlParm.js";
import { request } from "https";
export default {
  name: "MyAiguxuan",
  data() {
    return {
      more: {
        name: [
          "优惠券",
          "我的积分",
          "我的收藏",
          "足迹",
          "我的拍卖",
          "我的钱包",
          "我的发票",
          "我要开店",
          "我的评价",
          "收货地址",
          "客服中心",
          "意见反馈",
          "套餐订单",
          "积分订单",
          "分销中心",
          "举报"
        ],
        icon: [
          require("@/assets/images/coupon.png"),
          require("@/assets/images/intmall.png"),
          require("@/assets/images/collection.png"),
          require("@/assets/images/footprint.png"),
          require("@/assets/images/auction.png"),
          require("@/assets/images/wallet.png"),
          require("@/assets/images/invoice.png"),
          require("@/assets/images/shop.png"),
          require("@/assets/images/asses.png"),
          require("@/assets/images/myAdvice.png"),
          require("@/assets/images/service.png"),
          require("@/assets/images/opinion.png"),
          require("@/assets/images/package-0.png"),
          require("@/assets/images/intmall_order.png"),
          require("@/assets/images/fenxiaozhongxin.png"),
          require("@/assets/images/report1.png")
        ]
      },
      scrollWatch: false,
      Imag: "",
      navCon: [
        { text: "待付款", imgData: require("@/assets/Orders_01.png") },
        { text: "待处理", imgData: require("@/assets/Orders_02.png") },
        { text: "待收货", imgData: require("@/assets/Orders_03.png") },
        { text: "已完成", imgData: require("@/assets/Orders_04.png") },
        { text: "返修/退货", imgData: require("@/assets/Orders_05.png") }
      ],
      data: "",
      user_header: true
    };
  },
  destroyed() {
    this.scrollWatch = false;
  },
  components: {
    Shopsn
  },
  methods: {
    toOrder_list() {
      this.$router.push({
        name: "orderList",
        params: { id: 0 }
      });
    },
    toOrder(sat) {
      if (sat == 0) {
        this.$router.push({
          name: "orderList",
          params: { id: 1 }
        });
      } else if (sat == 1) {
        this.$router.push({
          name: "orderList",
          params: { id: 2 }
        });
      } else if (sat == 2) {
        this.$router.push({
          name: "orderList",
          params: { id: 3 }
        });
      } else if (sat == 3) {
        this.$router.push({
          name: "orderList",
          params: { id: 4 }
        });
      } else if (sat == 4) {
        this.$router.push("/repair");
        return;
      }
    },
    toLink(link) {
      function isString(str) {
        return typeof str == "string" && str.constructor == String;
      }
      if (isString(link)) {
        this.$router.push({
          path: link
        });
      } else {
        switch (link) {
          case 0:
            this.$router.push("/coupon");
            break;
          case 1:
            this.$router.push("/Integral");
            break;
          case 2:
            this.$router.push("/Collection");
            break;
          case 3:
            this.$router.push("/footprint");
            break;
          case 5:
            this.$router.push("/myWallet");
            break;
          case 6:
            this.$router.push("/documents");
            break;
          case 7:
            this.$router.push("/shopOpen");
            break;
          case 8:
            this.$router.push("/myComment");
            break;
          case 9:
            this.$router.push({
              name: "address",
              params: {
                status: 2
              }
            });
            break;
          case 10:
            this.$router.push("/customer");
            break;
          case 11:
            this.$router.push("/Feedback");
            break;
          case 12:
            this.$router.push({
              name: "packageList",
              params: {
                id: 0
              }
            });
            break;
          case 13:
            this.$router.push("/intOrder");
            break;
          case 14:
            this.$router.push("/distribution");
            break;
          case 15:
            this.$router.push("/informOutline");
            break;
        }
      }
    }
  },
  created() {},
  mounted() {
    document.body.scrollTop = 0;
    this.axios
      .post(this.$httpConfig.userinfo)
      .then(res => {
        if (res.data.status == 10001) {
          this.$router.push("/LogIn");
        } else {
          this.data = res.data.data;
          let ua = window.navigator.userAgent.toLowerCase();
          if (ua.match(/MicroMessenger/i) == "micromessenger") {
            this.$store.state.user_Imag = res.data.data.weixheader;
          } else {
            this.$store.state.user_Imag = res.data.data.user_header;
          }
        }
      })
      .catch(err => {
        console.log(err);
      });
  },
  wacth: {}
};
</script>
<style lang="less" scoped>
.per-center {
  background: #f1f1f1;
}
.header-wrap {
  width: 100%;
  background: -webkit-linear-gradient(left, #d02629, #d02629);
  background: -o-linear-gradient(right, #d02629, #d02629);
  background: -moz-linear-gradient(right, #d02629, #d02629);
  background: linear-gradient(to right, #d02629, #d02629);
  .header-hd {
    height: 0.6rem;
    line-height: 0.6rem;
    font-size: 0.32rem;
    padding: 0.2rem 0;
    color: #fff;
    text-align: center;
    position: relative;
    span {
      width: 0.47rem;
      height: 0.46rem;
      position: absolute;
      right: 0.23rem;
      top: 50%;
      margin-top: -0.23rem;
      background: url("../../assets/news1.png") no-repeat;
      background-size: 100% 100%;
    }
  }
  .header-dd-center {
    width: 100%;
    height: 1.5rem;
    position: relative;
    .setUp {
      width: 0.62rem;
      height: 0.61rem;
      position: absolute;
      left: 1.04rem;
      top: 50%;
      margin-top: -0.305rem;
      background: url(../../assets/setUp.png) no-repeat;
      background-size: 100% 100%;
    }
    .icon-wrap {
      width: 1.26rem;
      height: 1.26rem;
      border: 0.09rem solid #e24242;
      border-radius: 100%;
      overflow: hidden;
      position: absolute;
      left: 0;
      top: 0;
      right: 0;
      bottom: 0;
      margin: auto;
      img {
        width: 1.26rem;
        height: 1.26rem;
        border-radius: 100%;
        box-sizing: border-box;
      }
    }
    .admin {
      height: 0.61rem;
      width: 1.68rem;
      line-height: 0.61rem;
      background: #fff;
      position: absolute;
      right: 0;
      top: 50%;
      margin-top: -0.305rem;
      border-radius: 20px 0 0 20px;
      box-sizing: border-box;
      text-indent: 0.53rem;
      font-size: 0.24rem;
      color: #797979;
      span {
        position: absolute;
        top: 50%;
        left: 0.15rem;
        margin-top: -0.135rem;
        width: 0.27rem;
        height: 0.27rem;
        background: url(../../assets/images/zh.png) no-repeat;
        background-size: 100% 100%;
      }
    }
  }
  .status {
    padding: 0.2rem 0;
    text-align: center;
    line-height: 0.31rem;
    span {
      display: inline-block;
      height: 0.31rem;
    }
    .name {
      font-size: 0.28rem;
      color: #fff;
    }
    .member {
      padding-left: 0.37rem;
      padding-right: 0.1rem;
      width: 1.08rem;
      font-size: 0.24rem;
      color: #fff600;
      background: url(../../assets/mberIcon.png) no-repeat;
      background-size: 100% 100%;
    }
  }
}
.myOrder {
  width: 7.1rem;
  height: 0.94rem;
  padding: 0 0.2rem;
  line-height: 0.94rem;
  background: #fff;
  .title {
    font-size: 0.28rem;
    color: #333;
    text-indent: 0.6rem;
    position: relative;
  }
  .icon {
    position: absolute;
    left: 0;
    top: 50%;
    width: 0.39rem;
    height: 0.39rem;
    margin-top: -0.195rem;
    background: url(../../assets/images/qddd.png) no-repeat;
    background-size: 100% 100%;
  }
  .icon-wallet {
    position: absolute;
    left: 0;
    top: 50%;
    width: 0.39rem;
    height: 0.39rem;
    margin-top: -0.195rem;
    background: url(../../assets/wallet_ico.png) no-repeat;
    background-size: 100% 100%;
  }
  .see {
    font-size: 0.24rem;
    color: #999;
    position: relative;
    padding-right: 0.4rem;
    .icon-right {
      position: absolute;
      right: 0;
      top: 50%;
      width: 0.18rem;
      height: 0.3rem;
      margin-top: -0.15rem;
      background: url("../../assets/btn-right.png") no-repeat;
      background-size: 100% 100%;
    }
  }
}
.nav-link {
  margin-bottom: 0.14rem;
  background: #fff;
  li {
    width: 20%;
    height: 1.26rem;
    text-align: center;
    padding-top: 0.1rem;
    .icon-wrap {
      width: 100%;
      height: 0.8rem;
      position: relative;
      img {
        position: absolute;
        left: 0;
        top: 0;
        bottom: 0;
        right: 0;
        margin: auto;
      }
    }
    p {
      font-size: 0.24rem;
      color: #797979;
    }
  }
  li:nth-child(1) {
    .icon-wrap {
      img {
        width: 0.39rem;
        height: 0.38rem;
      }
    }
  }
  li:nth-child(2) {
    .icon-wrap {
      img {
        width: 0.41rem;
        height: 0.4rem;
      }
    }
  }
  li:nth-child(3) {
    .icon-wrap {
      img {
        width: 0.39rem;
        height: 0.37rem;
      }
    }
  }
  li:nth-child(4) {
    .icon-wrap {
      img {
        width: 0.4rem;
        height: 0.34rem;
      }
    }
  }
  li:nth-child(5) {
    .icon-wrap {
      img {
        width: 0.43rem;
        height: 0.37rem;
      }
    }
  }
}
.account {
  padding: 0.2rem 0;
  height: 0.95rem;
  width: 100%;
  background: #fff;
  margin-bottom: 0.14rem;
  li {
    width: 50%;
    height: 100%;
    border-right: 1px solid #e7e7e7;
    box-sizing: border-box;
    text-align: center;
    .number {
      font-size: 0.42rem;
      color: #333;
      .money {
        font-size: 0.32rem;
        color: #999;
      }
      .min {
        font-size: 0.32rem;
      }
    }
    .balance {
      font-size: 0.24rem;
      color: #999;
      padding-top: 0.15rem;
    }
  }
  li:last-child {
    border: none;
  }
}
.more {
  border-top: 1px solid #dfdfdf;
  background: #fff;
  li:nth-child(15) {
    .icon-main {
      img {
        display: inline-block;
        width: 0.68rem;
      }
    }
  }
  li:nth-child(16) {
    .icon-main {
      img {
        display: inline-block;
        width: 0.68rem;
      }
    }
  }
  li {
    width: 25%;
    height: 2rem;
    border-right: 1px solid #ebebeb;
    border-bottom: 1px solid #ebebeb;
    box-sizing: border-box;
    text-align: center;
    p {
      font-size: 0.24rem;
      color: #333;
    }
    .icon-main {
      width: 100%;
      height: 0.86rem;
      position: relative;
      padding-top: 0.57rem;
    }
    &:nth-child(1) {
      img {
        width: 0.7rem;
        height: 0.51rem;
      }
    }
    &:nth-child(2) {
      img {
        width: 0.49rem;
        height: 0.51rem;
      }
    }
    &:nth-child(3) {
      img {
        width: 0.46rem;
        height: 0.5rem;
      }
    }
    &:nth-child(4) {
      img {
        width: 0.45rem;
        height: 0.55rem;
      }
    }
    &:nth-child(5) {
      img {
        width: 0.67rem;
        height: 0.61rem;
      }
    }
    &:nth-child(6) {
      img {
        width: 0.52rem;
        height: 0.5rem;
      }
    }
    &:nth-child(7) {
      img {
        width: 0.54rem;
        height: 0.43rem;
      }
    }
    &:nth-child(8) {
      img {
        width: 0.5rem;
        height: 0.49rem;
      }
    }
    &:nth-child(9) {
      img {
        width: 0.53rem;
        height: 0.53rem;
      }
    }
    &:nth-child(10) {
      img {
        width: 0.58rem;
        height: 0.52rem;
      }
    }
    &:nth-child(11) {
      img {
        width: 0.61rem;
        height: 0.57rem;
      }
    }
    &:nth-child(12) {
      img {
        width: 0.48rem;
        height: 0.48rem;
      }
    }
    &:nth-child(13) {
      img {
        width: 0.6rem;
        height: 0.61rem;
      }
    }
    &:nth-child(14) {
      img {
        width: 0.7rem;
        height: 0.72rem;
      }
    }
    &:nth-child(4n) {
      border-right: none;
    }
  }
}
</style>