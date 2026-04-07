<template>
  <div class="auth-page auth-page--login">
    <div class="auth-shell">
      <div class="auth-hero">
        <span class="auth-badge">Smart Commerce</span>
        <h1>欢迎回来</h1>
        <p>登录后即可继续浏览商品、查看订单和管理个人购物信息。</p>
        <div class="auth-feature-list">
          <div class="auth-feature">更清晰的购物入口</div>
          <div class="auth-feature">更舒服的浏览体验</div>
          <div class="auth-feature">更统一的商城视觉</div>
        </div>
      </div>

      <div class="auth-card">
        <div class="auth-title">登录商城</div>
        <div class="auth-subtitle">输入账号信息，继续你的购物流程</div>

        <el-form :model="form" :rules="rules" ref="formRef" class="auth-form">
          <el-form-item prop="username">
            <el-input prefix-icon="el-icon-user" placeholder="请输入账号" v-model="form.username"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input
              prefix-icon="el-icon-lock"
              placeholder="请输入密码"
              show-password
              v-model="form.password"
            ></el-input>
          </el-form-item>
          <el-form-item prop="role">
            <el-select v-model="form.role" placeholder="请选择角色" style="width: 100%">
              <el-option label="管理员" value="ADMIN"></el-option>
              <el-option label="商家" value="BUSINESS"></el-option>
              <el-option label="用户" value="USER"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button class="auth-submit" type="primary" @click="login">登录</el-button>
          </el-form-item>
          <div class="auth-switch">
            <span>还没有账号？</span>
            <a href="/register">立即注册</a>
          </div>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      form: {},
      rules: {
        username: [{ required: true, message: "请输入账号", trigger: "blur" }],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
        role: [{ required: true, message: "请选择角色", trigger: "change" }],
      },
    };
  },
  methods: {
    login() {
      this.$refs.formRef.validate((valid) => {
        if (!valid) {
          return;
        }
        this.$request.post("/login", this.form).then((res) => {
          if (res.code === "200") {
            const user = res.data;
            localStorage.setItem("xm-user", JSON.stringify(user));
            if (user.role === "USER") {
              location.href = "/front/home";
            } else {
              location.href = "/home";
            }
            this.$message.success("登录成功");
          } else {
            this.$message.error(res.msg);
          }
        });
      });
    },
  },
};
</script>

<style scoped>
.auth-page {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 32px 16px;
  background:
    linear-gradient(135deg, rgba(11, 18, 36, 0.75), rgba(20, 33, 61, 0.3)),
    url("@/assets/imgs/bg.jpg") center/cover no-repeat;
}

.auth-shell {
  width: min(1120px, 100%);
  display: grid;
  grid-template-columns: minmax(0, 1.1fr) minmax(380px, 0.9fr);
  border-radius: 32px;
  overflow: hidden;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.18);
  backdrop-filter: blur(18px);
  box-shadow: 0 28px 80px rgba(15, 23, 42, 0.28);
}

.auth-hero,
.auth-card {
  padding: 48px;
}

.auth-hero {
  color: #f8fafc;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: linear-gradient(180deg, rgba(11, 18, 36, 0.2), rgba(11, 18, 36, 0.45));
}

.auth-badge {
  align-self: flex-start;
  padding: 8px 14px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.12);
  font-size: 12px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}

.auth-hero h1 {
  margin-top: 18px;
  font-size: clamp(38px, 5vw, 58px);
  line-height: 1.04;
}

.auth-hero p {
  margin-top: 16px;
  max-width: 420px;
  color: rgba(248, 250, 252, 0.82);
  line-height: 1.8;
  font-size: 16px;
}

.auth-feature-list {
  margin-top: 28px;
  display: grid;
  gap: 12px;
}

.auth-feature {
  display: inline-flex;
  align-self: flex-start;
  padding: 12px 16px;
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.1);
}

.auth-card {
  background: rgba(255, 255, 255, 0.94);
}

.auth-title {
  font-size: 32px;
  font-weight: 700;
  color: var(--text-primary);
}

.auth-subtitle {
  margin-top: 10px;
  color: var(--text-secondary);
}

.auth-form {
  margin-top: 28px;
}

.auth-submit {
  width: 100%;
  min-height: 46px;
}

.auth-switch {
  display: flex;
  justify-content: flex-end;
  gap: 6px;
  color: var(--text-secondary);
}

.auth-switch a {
  color: var(--brand);
  font-weight: 700;
}

@media (max-width: 900px) {
  .auth-shell {
    grid-template-columns: 1fr;
  }

  .auth-hero,
  .auth-card {
    padding: 28px;
  }
}
</style>
