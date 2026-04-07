<template>
  <div class="type-page">
    <div class="type-shell section-card">
      <div class="type-main">
        <div class="type-header">
          <div>
            <div class="type-title">{{ typeData.name }}</div>
            <div class="type-subtitle">当前分类下的精选商品</div>
          </div>
          <el-button icon="el-icon-arrow-left" @click="goBack">返回上一页</el-button>
        </div>

        <div class="goods-grid">
          <div v-for="item in goodsData" :key="item.id" class="goods-card" @click="navTo('/front/detail?id=' + item.id)">
            <img :src="item.img" alt="goods" class="goods-image">
            <div class="goods-name">{{ item.name }}</div>
            <div class="goods-price">￥{{ item.price }} / {{ item.unit }}</div>
          </div>
        </div>
      </div>

      <div class="type-side">
        <div class="side-title">猜你喜欢</div>
        <div class="recommend-list">
          <div v-for="item in recommendData" :key="item.id" class="recommend-card" @click="navTo('/front/detail?id=' + item.id)">
            <img :src="item.img" alt="recommend" class="recommend-image">
            <div class="recommend-content">
              <div class="goods-name">{{ item.name }}</div>
              <div class="goods-price">￥{{ item.price }} / {{ item.unit }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    const typeId = this.$route.query.id;
    return {
      user: JSON.parse(localStorage.getItem("xm-user") || "{}"),
      typeId,
      goodsData: [],
      recommendData: [],
      typeData: {},
    };
  },
  mounted() {
    this.loadGoods();
    this.loadType();
    this.loadRecommend();
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
      this.$request.get("/type/selectById/" + this.typeId).then((res) => {
        if (res.code === "200") {
          this.typeData = res.data;
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    loadGoods() {
      this.$request.get("/goods/selectByTypeId?id=" + this.typeId).then((res) => {
        if (res.code === "200") {
          this.goodsData = res.data;
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    navTo(url) {
      window.location.href = url;
    },
    goBack() {
      if (window.history.length > 1) {
        this.$router.back();
      } else {
        this.$router.push("/front/home");
      }
    },
  },
};
</script>

<style scoped>
.type-page {
  padding-bottom: 24px;
}

.type-shell {
  width: min(1240px, 100%);
  margin: 0 auto;
  padding: 24px;
  display: grid;
  grid-template-columns: minmax(0, 1fr) 180px;
  gap: 20px;
}

.type-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding-bottom: 20px;
  margin-bottom: 20px;
  border-bottom: 1px solid var(--line-soft);
}

.type-title,
.side-title {
  font-size: 24px;
  font-weight: 700;
  color: var(--text-primary);
}

.type-subtitle {
  margin-top: 8px;
  color: var(--text-secondary);
}

.goods-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 16px;
}

.goods-card,
.recommend-card {
  padding: 14px;
  border-radius: 20px;
  border: 1px solid var(--line-soft);
  background: linear-gradient(180deg, #ffffff 0%, #fbfcfe 100%);
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
}

.recommend-card {
  padding: 8px;
  border-radius: 16px;
  display: flex;
  align-items: center;
  gap: 10px;
  width: 100%;
  max-width: 100%;
  overflow: hidden;
}

.goods-card:hover,
.recommend-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-sm);
  border-color: rgba(255, 90, 54, 0.18);
}

.goods-image {
  width: 100%;
  height: 175px;
  object-fit: cover;
  border-radius: 16px;
}

.recommend-image {
  width: 64px;
  min-width: 64px;
  height: 64px;
  object-fit: cover;
  border-radius: 12px;
}

.goods-name {
  margin-top: 12px;
  font-size: 15px;
  font-weight: 600;
  line-height: 1.5;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.goods-price {
  margin-top: 8px;
  font-size: 20px;
  font-weight: 700;
  color: var(--brand);
}

.recommend-card .goods-name {
  font-size: 14px;
}

.recommend-card .goods-price {
  font-size: 18px;
}

.type-side {
  padding-left: 20px;
  border-left: 1px solid var(--line-soft);
}

.recommend-list {
  margin-top: 16px;
  display: grid;
  gap: 12px;
}

.recommend-content {
  min-width: 0;
  flex: 1;
}

.recommend-card .goods-name {
  font-size: 13px;
  line-height: 1.4;
  white-space: normal;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.recommend-card .goods-price {
  margin-top: 6px;
  font-size: 16px;
}

@media (max-width: 960px) {
  .type-shell {
    grid-template-columns: 1fr;
  }

  .type-side {
    padding-left: 0;
    border-left: none;
    border-top: 1px solid var(--line-soft);
    padding-top: 24px;
  }

  .goods-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 640px) {
  .type-shell {
    padding: 18px;
  }

  .type-header {
    align-items: flex-start;
    flex-direction: column;
  }

  .goods-grid {
    grid-template-columns: 1fr;
  }
}
</style>
