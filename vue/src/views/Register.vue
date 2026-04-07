<template>
  <div class="auth-page auth-page--register">
    <div class="auth-shell">
      <div class="auth-hero">
        <span class="auth-badge">Create Account</span>
        <h1>注册新账号</h1>
        <p>选择合适的角色后即可开始使用商城，用户和商家都能快速进入自己的工作流。</p>
        <div class="auth-stat-list">
          <div class="auth-stat">
            <strong>2</strong>
            <span>种可注册角色</span>
          </div>
          <div class="auth-stat">
            <strong>1</strong>
            <span>步完成开通</span>
          </div>
        </div>
      </div>

      <div class="auth-card">
        <div class="auth-title">创建账号</div>
        <div class="auth-subtitle">填写基础信息后即可立即登录使用</div>

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
          <el-form-item prop="confirmPass">
            <el-input
              prefix-icon="el-icon-lock"
              placeholder="请再次确认密码"
              show-password
              v-model="form.confirmPass"
            ></el-input>
          </el-form-item>
          <el-form-item prop="role">
            <el-select v-model="form.role" placeholder="请选择角色" style="width: 100%">
              <el-option label="商家" value="BUSINESS"></el-option>
              <el-option label="用户" value="USER"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button class="auth-submit" type="primary" @click="register">注册</el-button>
          </el-form-item>
          <div class="auth-switch">
            <span>已有账号？</span>
            <a href="/login">前往登录</a>
          </div>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Register",
  data() {
    const validatePassword = (rule, confirmPass, callback) => {
      if (confirmPass === "") {
        callback(new Error("请再次确认密码"));
      } else if (confirmPass !== this.form.password) {
        callback(new Error("两次输入的密码不一致"));
      } else {
        callback();
      }
    };
    return {
      form: {},
      rules: {
        username: [{ required: true, message: "请输入账号", trigger: "blur" }],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
        role: [{ required: true, message: "请选择角色", trigger: "change" }],
        confirmPass: [{ validator: validatePassword, trigger: "blur" }],
      },
    };
  },
  methods: {
    register() {
      this.$refs.formRef.validate((valid) => {
        if (!valid) {
          return;
        }
        this.$request.post("/register", this.form).then((res) => {
          if (res.code === "200") {
            this.$router.push("/login");
            this.$message.success("注册成功");
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
    linear-gradient(135deg, rgba(15, 23, 42, 0.68), rgba(15, 118, 110, 0.26)),
    url("@/assets/imgs/bg1.jpg") center/cover no-repeat;
}

.auth-shell {
  width: min(1120px, 100%);
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(400px, 0.92fr);
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
  background: linear-gradient(180deg, rgba(15, 118, 110, 0.14), rgba(11, 18, 36, 0.38));
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
  font-size: clamp(36px, 5vw, 54px);
  line-height: 1.06;
}

.auth-hero p {
  margin-top: 14px;
  max-width: 420px;
  color: rgba(248, 250, 252, 0.82);
  line-height: 1.8;
  font-size: 16px;
}

.auth-stat-list {
  margin-top: 28px;
  display: flex;
  gap: 14px;
  flex-wrap: wrap;
}

.auth-stat {
  min-width: 140px;
  padding: 18px 20px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.1);
}

.auth-stat strong {
  display: block;
  font-size: 28px;
  line-height: 1;
}

.auth-stat span {
  display: block;
  margin-top: 8px;
  color: rgba(248, 250, 252, 0.78);
}

.auth-card {
  background: rgba(255, 255, 255, 0.95);
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
  color: var(--accent);
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
