<template>
  <div class="home-container">
    <van-sticky>
      <van-search v-model="address" show-action label="地址" shape="round" background="skyblue" placeholder="请输入目的地址"
        @search="onSearch">
        <template #action>
          <div style="color:white" @click="onSearch">搜索</div>
        </template>
      </van-search>
    </van-sticky>
    <!-- /导航栏 -->
    <!-- 轮播图 -->
    <van-swipe class="my-swipe" :autoplay="3000">
      <van-swipe-item v-for="(image, index) in swipeImages" :key="index">
        <img v-lazy="image" />
      </van-swipe-item>
    </van-swipe>
    <!-- /轮播图 -->
    <van-divider>快捷功能</van-divider>
    <!-- 快捷功能 -->
    <van-grid clickable :column-num="3" :gutter="10">
      <van-grid-item icon="location-o" text="自助停车" to="/parklots" />
      <van-grid-item icon="idcard" text="车牌管理" to="/user/car" />
      <van-grid-item icon="scan" text="快速缴费" to="/" />
      <van-grid-item icon="paid" text="快速充值" to="/user/charge" />
      <van-grid-item icon="pending-payment" text="我的钱包" to="/user/wallet" />
      <van-grid-item icon="service-o" text="在线反馈" to="/user/chat" />
    </van-grid>
    <!-- /快捷功能 -->
    <van-divider>附近停车场</van-divider>
    <!-- 推荐停车场 -->
    <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="onLoad">
      <van-card tag="推荐" price="5.00" desc="距您460m,步行需要5分钟" title="黄金商业广场停车场" :thumb="parklotImages[0]" @click="parklotDetail">
        <template #tags>
          <van-tag plain type="warning ">离你最近</van-tag>
          <van-tag plain type="warning ">半小时内免费</van-tag>
        </template>
      </van-card>
      <van-card tag="推荐" price="4.00" desc="距您790m,步行需要9分钟" title="西山上筑停车场" :thumb="parklotImages[1]">
        <template #tags>
          <van-tag plain type="warning ">价格最低</van-tag>
          <van-tag plain type="warning ">一小时内免费</van-tag>
        </template>
      </van-card>
      <van-card price="5.00" desc="距您1.6km,驾车需要7分钟" title="明日广场露天停车场" :thumb="parklotImages[2]">
      </van-card>
      <van-card price="5.00" desc="距您2.0km,驾车需要9分钟" title="永旺购物中心停车场" :thumb="parklotImages[3]">
        <template #tags>
          <van-tag plain type="warning ">剩余车位多</van-tag>
        </template>
      </van-card>
      <van-card price="8.00" desc="距您2.4km,驾车需要10分钟" title="步行街停车场" :thumb="parklotImages[4]">
        <template #tags>
          <van-tag plain type="warning ">休闲娱乐</van-tag>
        </template>
      </van-card>
    </van-list>
    <!-- 推荐停车场 -->
    <van-divider>没有更多了</van-divider>
  </div>
</template>

<script>
  import {
    mapState
  } from 'vuex'
  // vant 懒加载

  export default {
    name: 'HomeIndex',
    components: {},
    props: {},
    data() {
      return {
        address: '',
        swipeImages: [
          require('./assets/swipe1.jpg'),
          require('./assets/swipe2.jpg'),
          require('./assets/swipe3.jpg')
        ],
        loading: false,
        finished: true,
        parklotImages: [
          require('./assets/parklot1.jpg'),
          require('./assets/parklot2.jpg'),
          require('./assets/parklot3.jpg'),
          require('./assets/parklot4.jpg'),
          require('./assets/parklot5.jpg')
        ],
        parklotId: 1
      }
    },
    computed: {
      ...mapState(['user'])
    },
    watch: {
      user() {

      }
    },
    created() {},
    mounted() {},
    methods: {
      onSearch() {
      },
      onLoad() {
      },
      parklotDetail() {
        console.log(`/parklots/${this.parklotId}`)
        this.$router.push(`/parklots/${this.parklotId}`)
      }
    }
  }
</script>

<style scoped lang="less">
  .my-swipe {
    width: 100vw;
    height: 200px;

    /deep/.van-swipe-item img {
      max-width: 100%;
    }
  }
</style>
