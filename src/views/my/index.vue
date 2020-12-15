<template>
  <div class="my-container">
    <van-cell-group v-if="user" class="my-info">
      <van-cell class="base-info" center :border="false">
        <van-image class="avatar" slot="icon" round fit="cover" :src="currentUser.photo" />
        <div class="name" slot="title">{{ currentUser.name }}</div>
        <van-button class="update-btn" size="small" round to="/user/profile">编辑资料</van-button>
      </van-cell>
      <van-grid class="data-info" :border="false" :column-num="3">
        <van-grid-item class="data-info-item">
          <div slot="text" class="text-wrap">
            <div class="count">{{ userInfo.parkTime }}</div>
            <div class="text">已停时长</div>
          </div>
        </van-grid-item>
        <van-grid-item class="data-info-item">
          <div slot="text" class="text-wrap">
            <div class="count">{{ userInfo.paid }}</div>
            <div class="text">预计缴费</div>
          </div>
        </van-grid-item>
        <van-grid-item class="data-info-item">
          <div slot="text" class="text-wrap">
            <div class="count">{{ userInfo.account }}</div>
            <div class="text">账户余额</div>
          </div>
        </van-grid-item>

      </van-grid>
    </van-cell-group>

    <div v-else class="not-login">
      <div @click="$router.push({
        name: 'login',
        query: {
          redirect: '/my'
        }
      })">
        <img class="mobile" src="./手机.png">
      </div>
      <div class="text">登录 / 注册</div>
    </div>

    <van-grid class="nav-grid mb-4" :column-num="3">
      <van-grid-item class="nav-grid-item" icon="orders-o" text="全部订单" to="/user/order" />
      <van-grid-item class="nav-grid-item" icon="pending-payment" text="我的钱包" to="/user/wallet" />
      <van-grid-item class="nav-grid-item" icon="idcard" text="我的车牌" to="/user/car" />
    </van-grid>
    <!-- 优惠券单元格 -->
    <van-coupon-cell :coupons="coupons" :chosen-coupon="chosenCoupon" @click="showList = true" />
    <van-cell title="消息通知" is-link to="/" />
    <van-cell title="使用帮助" is-link to="/" />
    <van-cell class="mb-4" title="在线反馈" is-link to="/user/chat" />
    <van-cell v-if="user" class="logout-cell" title="退出登录" @click="onLogout" />
    <!-- 优惠券列表 -->
    <van-popup v-model="showList" round position="bottom" style="height: 90%; padding-top: 4px;">
      <van-coupon-list :coupons="coupons" :chosen-coupon="chosenCoupon" :disabled-coupons="disabledCoupons" @change="onChange"
        @exchange="onExchange" />
    </van-popup>
    <!-- /优惠券列表 -->
  </div>

</template>

<script>
  import {
    mapState
  } from 'vuex'
  import {
    getCurrentUser
  } from '@/api/user'
  const coupon = {
    available: 1,
    condition: '无使用门槛\n最多优惠5元',
    reason: '',
    value: 150,
    name: '优惠券名称',
    startAt: 1489104000,
    endAt: 1514592000,
    valueDesc: '1.5',
    unitDesc: '元'
  }
  export default {
    name: 'MyIndex',
    components: {},
    props: {},
    data() {
      return {
        currentUser: {}, // 当前登录用户信息
        userInfo: {
          parkTime: '1:40:50',
          paid: '￥8.00',
          account: '￥20.00'
        },
        showList: false,
        chosenCoupon: -1,
        coupons: [coupon],
        disabledCoupons: [coupon]
      }
    },
    computed: {
      ...mapState(['user'])
    },
    watch: {},
    created() {},
    mounted() {},
    activated() {
      if (this.user) {
        this.loadCurrentUser()
      }
    },
    methods: {
      async loadCurrentUser() {
        const {
          data
        } = await getCurrentUser()
        this.currentUser = data.data
      },

      onLogout() {
        // 提示用户确认退出
        // 确认 -> 处理退出
        this.$dialog.confirm({
            title: '退出提示',
            message: '确认退出吗？'
          })
          .then(() => { // 确认执行这里
            // 清除用户登录状态
            this.$store.commit('setUser', null)
          })
          .catch(() => { // 退出执行这里
            // on cancel
          })
      },
      onChange(index) {
        this.showList = false
        this.chosenCoupon = index
      },
      onExchange(code) {
        this.coupons.push(coupon)
      }
    }
  }
</script>

<style scoped lang="less">
  .my-container {
    .my-info {
      background-color: skyblue;
      background-size: cover;

      .base-info {
        box-sizing: border-box;
        height: 115px;
        background-color: unset;
        padding-top: 38px;
        padding-bottom: 11px;

        .avatar {
          box-sizing: border-box;
          width: 66px;
          height: 66px;
          border: 1px solid #fff;
          margin-right: 11px;
        }

        .name {
          font-size: 15px;
          color: #fff;
        }

        .update-btn {
          height: 16px;
          font-size: 10px;
          color: #666666;
        }
      }

      .data-info {
        .data-info-item {
          height: 65px;
          color: #fff;

          .text-wrap {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;

            .count {
              font-size: 18px;
            }

            .text {
              font-size: 11px;
            }
          }
        }
      }

      /deep/ .van-grid-item__content {
        background-color: unset;
      }
    }

    .not-login {
      height: 180px;
      background: url("./banner.png") no-repeat;
      background-size: cover;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;

      .mobile {
        width: 66px;
        height: 66px;
      }

      .text {
        font-size: 14px;
        color: #fff;
      }
    }

    /deep/ .nav-grid {
      .nav-grid-item {
        height: 70px;

        .toutiao {
          font-size: 22px;
        }

        .toutiao-shoucang {
          color: #eb5253;
        }

        .toutiao-lishi {
          color: #ff9d1d;
        }

        .van-grid-item__text {
          font-size: 14px;
          color: #333333;
        }
      }
    }

    .logout-cell {
      text-align: center;
      color: #d86262;
    }

    .mb-4 {
      margin-bottom: 4px;
    }

    /deep/.van-button__text,/deep/.van-coupon__head{
      color: skyblue;
    }
    /deep/.van-tabs__line{
      background-color: skyblue;
    }
    /deep/.van-icon-success,/deep/ .van-coupon-list__bottom .van-button--danger{
      background-color: skyblue;
      border-color: skyblue;
    }
    /deep/.van-icon-success,/deep/.van-coupon-list__bottom .van-button__text{
      color: white;
    }
  }
</style>
