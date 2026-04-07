<template>
  <div class="main-content detail-page">
    <div class="detail-shell section-card">
      <div class="detail-top">
        <el-row :gutter="24">
          <el-col :span="12">
            <img :src="goodsData.img" alt="" class="detail-image">
          </el-col>
          <el-col :span="12">
            <div class="detail-title">{{ goodsData.name }}</div>
            <div class="detail-meta">销量：{{ goodsData.count }}</div>
            <div class="detail-price">抢购价：<span>{{ goodsData.price }} / {{ goodsData.unit }}</span></div>
            <div class="detail-banner-wrap">
              <img src="@/assets/imgs/right.png" alt="" class="detail-banner">
            </div>

            <div class="detail-meta">
              商家：<a href="#" @click.prevent="navTo('/front/business?id=' + goodsData.businessId)">{{ goodsData.businessName }}</a>
            </div>
            <div class="detail-meta">
              分类：<a href="#" @click.prevent="navTo('/front/type?id=' + goodsData.typeId)">{{ goodsData.typeName }}</a>
            </div>
            <div class="detail-actions">
              <el-button type="warning" @click="addCart">加入购物车</el-button>
              <el-button type="warning" @click="collect">收藏</el-button>
            </div>
          </el-col>
        </el-row>
      </div>

      <div class="detail-tabs-wrap">
        <el-tabs v-model="activeName" @tab-click="handleClick" class="detail-tabs">
          <el-tab-pane label="宝贝详情" name="first">
            <div class="detail-description" v-html="goodsData.description"></div>
          </el-tab-pane>
          <el-tab-pane label="宝贝评价" name="second">
            <div class="comment-list">
              <div class="comment-item" v-for="item in commentData" :key="item.id">
                <div class="comment-user">
                  <div class="comment-avatar">
                    <img :src="item.userAvatar" alt="">
                  </div>
                  <div class="comment-user-meta">
                    <div class="comment-name">{{ item.userName }}</div>
                    <div class="comment-time">{{ item.time }}</div>
                  </div>
                </div>
                <div class="comment-content">{{ item.content }}</div>
              </div>
            </div>
          </el-tab-pane>
        </el-tabs>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    const goodsId = this.$route.query.id;
    return {
      user: JSON.parse(localStorage.getItem("xm-user") || "{}"),
      goodsId,
      goodsData: {},
      commentData: [],
      activeName: "first",
    };
  },
  mounted() {
    this.loadGoods();
    this.loadComments();
  },
  methods: {
    loadGoods() {
      this.$request.get("/goods/selectById?id=" + this.goodsId).then((res) => {
        if (res.code === "200") {
          this.goodsData = res.data;
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    handleClick(tab) {
      this.activeName = tab.name;
    },
    collect() {
      const data = {
        userId: this.user.id,
        businessId: this.goodsData.businessId,
        goodsId: this.goodsId,
      };
      this.$request.post("/collect/add", data).then((res) => {
        if (res.code === "200") {
          this.$message.success("收藏成功");
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    addCart() {
      const data = {
        num: 1,
        userId: this.user.id,
        goodsId: this.goodsId,
        businessId: this.goodsData.businessId,
      };
      this.$request.post("/cart/add", data).then((res) => {
        if (res.code === "200") {
          this.$message.success("操作成功");
        } else {
          this.$message.error(res.msg);
        }
      });
    },
    navTo(url) {
      window.location.href = url;
    },
    loadComments() {
      this.$request.get("/comment/selectByGoodsId?id=" + this.goodsId).then((res) => {
        if (res.code === "200") {
          this.commentData = res.data;
        } else {
          this.$message.error(res.msg);
        }
      });
    },
  },
};
</script>

<style scoped>
.detail-page {
  padding-bottom: 24px;
}

.detail-shell {
  width: min(1240px, 100%);
  min-height: 1000px;
  margin: 20px auto;
  padding: 24px;
}

.detail-top {
  padding-bottom: 10px;
}

.detail-image {
  width: 100%;
  height: 420px;
  object-fit: cover;
  border-radius: 24px;
}

.detail-title {
  font-size: 28px;
  font-weight: 800;
  line-height: 1.45;
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.detail-meta {
  margin-top: 14px;
  color: var(--text-secondary);
}

.detail-price {
  margin-top: 18px;
  color: var(--brand);
  font-weight: 700;
}

.detail-price span {
  font-size: 28px;
}

.detail-banner-wrap {
  margin-top: 24px;
}

.detail-banner {
  width: 78%;
  height: 130px;
  object-fit: cover;
  border-radius: 18px;
}

.detail-actions {
  margin-top: 24px;
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.detail-tabs-wrap {
  margin-top: 16px;
}

.detail-tabs {
  width: 100%;
}

/deep/ .detail-tabs .el-tabs__header {
  margin-bottom: 22px;
}

/deep/ .detail-tabs .el-tabs__nav-wrap {
  padding: 0 4px;
}

/deep/ .detail-tabs .el-tabs__item {
  font-size: 16px;
  font-weight: 700;
  height: 46px;
  line-height: 46px;
}

.detail-description {
  padding: 10px 24px;
}

.comment-list {
  margin-top: 10px;
}

.comment-item + .comment-item {
  margin-top: 24px;
}

.comment-user {
  display: flex;
  align-items: center;
}

.comment-avatar img {
  width: 44px;
  height: 44px;
  object-fit: cover;
  border-radius: 50%;
}

.comment-user-meta {
  margin-left: 12px;
}

.comment-name {
  font-weight: 700;
  font-size: 16px;
}

.comment-time {
  margin-top: 4px;
  color: var(--text-muted);
}

.comment-content {
  margin-top: 14px;
  font-size: 15px;
  line-height: 1.8;
}

@media (max-width: 900px) {
  .detail-shell {
    padding: 18px;
  }

  .detail-image {
    height: 300px;
  }

  .detail-banner {
    width: 100%;
  }

  .detail-description {
    padding: 10px 0;
  }
}
</style>
