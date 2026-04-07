<template>
  <div class="home-page">
    <section class="hero-grid">
      <div class="hero-main section-card">
        <div class="hero-copy">
          <span class="hero-tag">New Season Picks</span>
          <h1>把日常购物变成一场更轻松、更有质感的挑选体验</h1>
          <p>
            从热门尖货到猜你喜欢，这里把高频购买场景整理成更清晰、更舒服的浏览方式。
          </p>
          <div class="hero-actions">
            <el-button type="primary" @click="scrollToSection('hot-sale')">查看热卖商品</el-button>
            <el-button @click="scrollToSection('guess-like')">逛猜你喜欢</el-button>
          </div>
        </div>

        <div class="hero-showcase">
          <div class="hero-metrics">
            <div class="metric-card">
              <strong>{{ goodsData.length || 15 }}+</strong>
              <span>今日热卖</span>
            </div>
            <div class="metric-card">
              <strong>{{ typeData.length || 8 }}+</strong>
              <span>精选分类</span>
            </div>
          </div>

          <el-carousel height="340px" :interval="4000">
            <el-carousel-item v-for="item in carouselTop" :key="item">
              <img :src="item" alt="banner" class="hero-banner">
            </el-carousel-item>
          </el-carousel>
        </div>
      </div>

      <div class="hero-side">
        <div class="user-panel section-card">
          <div class="user-panel__top">
            <img @click="navTo('/front/person')" :src="user.avatar" alt="avatar">
            <div>
              <div class="user-panel__label">Hi, {{ user.name || "欢迎回来" }}</div>
              <p>已为你整理常用入口和最新平台公告。</p>
            </div>
          </div>

          <div class="notice-card">
            <div class="notice-title">
              <i class="el-icon-bell"></i>
              <span>今日公告</span>
            </div>
            <div class="notice-content">{{ top || "新品上新中，快来发现本周热卖。" }}</div>
          </div>

          <div class="shortcut-grid">
            <a class="shortcut-item" href="#" @click.prevent="navTo('/front/collect')">
              <img src="@/assets/imgs/收藏.png" alt="collect">
              <span>我的收藏</span>
            </a>
            <a class="shortcut-item" href="#" @click.prevent="navTo('/front/address')">
              <img src="@/assets/imgs/店铺.png" alt="address">
              <span>我的地址</span>
            </a>
            <a class="shortcut-item" href="#" @click.prevent="navTo('/front/cart')">
              <img src="@/assets/imgs/购物车.png" alt="cart">
              <span>购物车</span>
            </a>
            <a class="shortcut-item" href="#" @click.prevent="navTo('/front/orders')">
              <img src="@/assets/imgs/订单.png" alt="orders">
              <span>我的订单</span>
            </a>
          </div>
        </div>

        <div class="promo-stack">
          <div class="promo-card section-card">
            <div>
              <span class="promo-tag">精选专场</span>
              <h3>爆款组合推荐</h3>
            </div>
            <img src="@/assets/imgs/right.png" alt="promo">
          </div>
        </div>
      </div>
    </section>

    <section class="category-panel section-card">
      <div class="section-heading">
        <div>
          <h2>逛逛热门分类</h2>
          <p>快速进入你最常浏览的商品区域</p>
        </div>
      </div>

      <div class="category-grid">
        <a
          v-for="item in typeData"
          :key="item.id"
          class="category-item"
          href="#"
          @click.prevent="navTo('/front/type?id=' + item.id)"
        >
          <img :src="item.img" alt="type">
          <span>{{ item.name }}</span>
        </a>
      </div>
    </section>

    <section class="story-grid">
      <div class="mini-carousel section-card">
        <div class="section-heading">
          <div>
            <h2>灵感左滑</h2>
            <p>用更轻的方式发现新商品</p>
          </div>
        </div>
        <el-carousel height="220px" :interval="4200">
          <el-carousel-item v-for="item in carouselLeft" :key="item">
            <img :src="item" alt="left-banner" class="story-image">
          </el-carousel-item>
        </el-carousel>
      </div>

      <div class="mini-carousel section-card">
        <div class="section-heading">
          <div>
            <h2>趋势右滑</h2>
            <p>把值得下单的内容放到更前面</p>
          </div>
        </div>
        <el-carousel height="220px" :interval="4200">
          <el-carousel-item v-for="item in carouselRight" :key="item">
            <img :src="item" alt="right-banner" class="story-image">
          </el-carousel-item>
        </el-carousel>
      </div>
    </section>

    <section id="hot-sale" class="goods-section section-card">
      <div class="section-heading">
        <div>
          <h2>热卖商品</h2>
          <p>高频下单、近期受欢迎的精选商品</p>
        </div>
      </div>
      <div class="goods-grid">
        <div v-for="item in goodsData" :key="item.id" class="goods-card" @click="navTo('/front/detail?id=' + item.id)">
          <div class="goods-cover">
            <img :src="item.img" alt="goods">
          </div>
          <div class="goods-content">
            <div class="goods-name">{{ item.name }}</div>
            <div class="goods-price">￥{{ item.price }} / {{ item.unit }}</div>
          </div>
        </div>
      </div>
    </section>

    <section id="guess-like" class="goods-section section-card">
      <div class="section-heading">
        <div>
          <h2>猜你喜欢</h2>
          <p>基于当前热度与推荐逻辑整理的商品列表</p>
        </div>
      </div>
      <div class="goods-grid">
        <div
          v-for="item in recommendData"
          :key="item.id"
          class="goods-card goods-card--soft"
          @click="navTo('/front/detail?id=' + item.id)"
        >
          <div class="goods-cover">
            <img :src="item.img" alt="recommend">
          </div>
          <div class="goods-content">
            <div class="goods-name">{{ item.name }}</div>
            <div class="goods-price">￥{{ item.price }} / {{ item.unit }}</div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: JSON.parse(localStorage.getItem("xm-user") || "{}"),
      typeData: [],
      top: "",
      notice: [],
      goodsData: [],
      recommendData: [],
      noticeTimer: null,
      carouselTop: [
        require("@/assets/imgs/carousel-1.png"),
        require("@/assets/imgs/carousel-2.png"),
        require("@/assets/imgs/carousel-9.png"),
      ],
      carouselLeft: [
        require("@/assets/imgs/carousel-3.png"),
        require("@/assets/imgs/carousel-4.png"),
        require("@/assets/imgs/carousel-5.png"),
      ],
      carouselRight: [
        require("@/assets/imgs/carousel-6.png"),
        require("@/assets/imgs/carousel-7.png"),
        require("@/assets/imgs/carousel-8.png"),
      ],
    };
  },
  mounted() {
    this.loadType();
    this.loadNotice();
    this.loadGoods();
    this.loadRecommend();
  },
  beforeDestroy() {
    if (this.noticeTimer) {
      clearInterval(this.noticeTimer);
    }
  },
  methods: {
    loadRecommend() {
      this.$request.get("/goods/recommend").then((res) => {
        if (res.code === "200") {
          this.recommendData = res.data;
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    loadType() {
      this.$request.get("/type/selectAll").then((res) => {
        if (res.code === "200") {
          this.typeData = res.data;
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    loadNotice() {
      this.$request.get("/notice/selectAll").then((res) => {
        this.notice = res.data || [];
        let index = 0;
        if (this.notice.length) {
          this.top = this.notice[0].content;
          if (this.noticeTimer) {
            clearInterval(this.noticeTimer);
          }
          this.noticeTimer = setInterval(() => {
            index = (index + 1) % this.notice.length;
            this.top = this.notice[index].content;
          }, 3000);
        }
      });
    },
    loadGoods() {
      this.$request.get("/goods/selectTop15").then((res) => {
        if (res.code === "200") {
          this.goodsData = res.data;
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    navTo(url) {
      if (url.indexOf("?") > -1) {
        window.location.href = url;
        return;
      }
      this.$router.push(url);
    },
    scrollToSection(id) {
      const el = document.getElementById(id);
      if (el) {
        el.scrollIntoView({ behavior: "smooth", block: "start" });
      }
    },
  },
};
</script>

<style scoped>
.home-page {
  display: flex;
  flex-direction: column;
  gap: 24px;
  padding-bottom: 24px;
}

.hero-grid {
  display: grid;
  grid-template-columns: minmax(0, 2fr) minmax(320px, 0.95fr);
  gap: 24px;
}

.hero-main {
  padding: 28px;
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(300px, 0.92fr);
  gap: 28px;
  overflow: hidden;
  position: relative;
}

.hero-main::before {
  content: "";
  position: absolute;
  inset: -40% auto auto -10%;
  width: 280px;
  height: 280px;
  background: radial-gradient(circle, rgba(255, 122, 89, 0.22), transparent 72%);
  pointer-events: none;
}

.hero-copy {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.hero-tag,
.promo-tag {
  display: inline-flex;
  align-self: flex-start;
  padding: 8px 14px;
  border-radius: 999px;
  background: rgba(255, 90, 54, 0.12);
  color: var(--brand);
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.hero-copy h1 {
  margin: 18px 0 14px;
  font-size: clamp(32px, 4vw, 52px);
  line-height: 1.06;
  max-width: 12ch;
}

.hero-copy p {
  max-width: 460px;
  color: var(--text-secondary);
  font-size: 16px;
  line-height: 1.8;
}

.hero-actions {
  margin-top: 24px;
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.hero-showcase {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.hero-metrics {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 14px;
}

.metric-card {
  padding: 18px;
  border-radius: 18px;
  background: linear-gradient(135deg, #fff6f2 0%, #ffffff 100%);
  border: 1px solid rgba(255, 90, 54, 0.1);
}

.metric-card strong {
  display: block;
  font-size: 28px;
  line-height: 1;
}

.metric-card span {
  display: block;
  margin-top: 8px;
  color: var(--text-secondary);
}

.hero-banner,
.story-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 20px;
}

.hero-side {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.user-panel,
.promo-card,
.category-panel,
.mini-carousel,
.goods-section {
  padding: 24px;
}

.user-panel__top {
  display: flex;
  align-items: center;
  gap: 14px;
}

.user-panel__top img {
  width: 72px;
  height: 72px;
  border-radius: 22px;
  object-fit: cover;
  cursor: pointer;
}

.user-panel__label {
  font-size: 22px;
  font-weight: 700;
}

.user-panel__top p {
  margin-top: 4px;
  color: var(--text-secondary);
  line-height: 1.6;
}

.notice-card {
  margin-top: 20px;
  padding: 18px;
  border-radius: 20px;
  background: linear-gradient(135deg, #172033 0%, #283653 100%);
  color: #f8fafc;
}

.notice-title {
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: 700;
}

.notice-content {
  margin-top: 12px;
  color: rgba(248, 250, 252, 0.86);
  line-height: 1.7;
}

.shortcut-grid {
  margin-top: 20px;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 12px;
}

.shortcut-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 14px;
  border-radius: 18px;
  background: var(--bg-soft);
  color: var(--text-primary);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.shortcut-item:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-sm);
}

.shortcut-item img {
  width: 24px;
  height: 24px;
}

.promo-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  min-height: 148px;
  background: linear-gradient(135deg, #eff6ff 0%, #ffffff 100%);
}

.promo-card h3 {
  margin-top: 10px;
  font-size: 24px;
}

.promo-card img {
  width: 120px;
  height: 120px;
  object-fit: cover;
  border-radius: 22px;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(6, minmax(0, 1fr));
  gap: 14px;
}

.category-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  padding: 18px 12px;
  border-radius: 20px;
  background: linear-gradient(180deg, #ffffff 0%, #f9fbff 100%);
  border: 1px solid var(--line-soft);
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
}

.category-item:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-sm);
  border-color: rgba(255, 90, 54, 0.18);
}

.category-item img {
  width: 42px;
  height: 42px;
  border-radius: 14px;
  object-fit: cover;
}

.category-item span {
  font-weight: 600;
  text-align: center;
}

.story-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 24px;
}

.goods-grid {
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  gap: 16px;
}

.goods-card {
  border-radius: 22px;
  overflow: hidden;
  background: linear-gradient(180deg, #ffffff 0%, #fbfcfe 100%);
  border: 1px solid var(--line-soft);
  cursor: pointer;
  transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease;
}

.goods-card:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-md);
  border-color: rgba(255, 90, 54, 0.18);
}

.goods-card--soft {
  background: linear-gradient(180deg, #fff8f5 0%, #ffffff 100%);
}

.goods-cover {
  padding: 14px;
}

.goods-cover img {
  width: 100%;
  height: 196px;
  object-fit: cover;
  border-radius: 18px;
}

.goods-content {
  padding: 0 16px 18px;
}

.goods-name {
  font-size: 16px;
  font-weight: 600;
  line-height: 1.5;
  min-height: 48px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.goods-price {
  margin-top: 10px;
  font-size: 22px;
  color: var(--brand);
  font-weight: 700;
}

@media (max-width: 1200px) {
  .hero-grid,
  .story-grid {
    grid-template-columns: 1fr;
  }

  .goods-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }

  .category-grid {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }
}

@media (max-width: 900px) {
  .hero-main {
    grid-template-columns: 1fr;
  }

  .goods-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .category-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }
}

@media (max-width: 640px) {
  .hero-main,
  .user-panel,
  .promo-card,
  .category-panel,
  .mini-carousel,
  .goods-section {
    padding: 18px;
  }

  .goods-grid,
  .category-grid,
  .shortcut-grid {
    grid-template-columns: 1fr;
  }

  .hero-actions .el-button {
    width: 100%;
  }
}
</style>
