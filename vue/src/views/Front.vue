<template>
  <div class="front-layout">
    <div class="front-notice glass-panel">
      <i class="el-icon-bell"></i>
      <span>平台公告</span>
      <span>{{ top || "欢迎来到天猫风格商城，发现更值得入手的好物。" }}</span>
    </div>

    <div class="front-header-wrap">
      <div class="front-header glass-panel">
        <div class="front-header-left" @click="navTo('/front/home')">
          <img src="@/assets/imgs/logo.png" alt="logo">
          <div class="front-brand">
            <span class="eyebrow">Online Mall</span>
            <div class="title">天猫风格购物商城</div>
          </div>
        </div>

        <div class="front-header-center">
          <div class="front-search">
            <el-input
              v-model="name"
              placeholder="搜索商品名称、分类或你想找的风格"
              clearable
              @keyup.enter.native="search"
            />
            <el-button type="primary" @click="search">搜索</el-button>
          </div>
        </div>

        <div class="front-header-right">
          <div v-if="!user.username" class="front-auth-actions">
            <el-button @click="$router.push('/login')">登录</el-button>
            <el-button type="primary" @click="$router.push('/register')">注册</el-button>
          </div>
          <div v-else>
            <el-dropdown>
              <div class="front-user-chip">
                <img @click="navTo('/front/person')" :src="user.avatar" alt="avatar">
                <div class="front-user-chip__text">
                  <span>{{ user.name || user.username }}</span>
                  <i class="el-icon-arrow-down"></i>
                </div>
              </div>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item>
                  <div @click="navTo('/front/cart')">我的购物车</div>
                </el-dropdown-item>
                <el-dropdown-item>
                  <div @click="navTo('/front/collect')">我的收藏</div>
                </el-dropdown-item>
                <el-dropdown-item>
                  <div @click="navTo('/front/address')">我的地址</div>
                </el-dropdown-item>
                <el-dropdown-item>
                  <div @click="navTo('/front/orders')">我的订单</div>
                </el-dropdown-item>
                <el-dropdown-item>
                  <div @click="logout">退出登录</div>
                </el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </div>
        </div>
      </div>
    </div>

    <div class="front-page-body">
      <router-view ref="child" @update:user="updateUser" />
    </div>
  </div>
</template>

<script>
export default {
  name: "FrontLayout",
  data() {
    return {
      top: "",
      notice: [],
      name: "",
      user: JSON.parse(localStorage.getItem("xm-user") || "{}"),
      noticeTimer: null,
    };
  },
  mounted() {
    this.loadNotice();
  },
  beforeDestroy() {
    if (this.noticeTimer) {
      clearInterval(this.noticeTimer);
    }
  },
  methods: {
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
    updateUser() {
      this.user = JSON.parse(localStorage.getItem("xm-user") || "{}");
    },
    navTo(url) {
      this.$router.push(url);
    },
    logout() {
      localStorage.removeItem("xm-user");
      this.$router.push("/login");
    },
    search() {
      this.$router.push({
        path: "/front/search",
        query: {
          name: this.name || "",
        },
      });
    },
  },
};
</script>

<style scoped>
@import "@/assets/css/front.css";
</style>
