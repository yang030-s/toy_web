<template>
  <div
    v-loading="loading"
    element-loading-text="登录中..."
    element-loading-spinner="el-icon-loading"
    element-loading-background="rgba(0, 0, 0, 0.6)"
    class="login-container"
  >
    <el-form
      ref="loginForm"
      v-if="!userReg"
      :model="loginForm"
      :rules="loginRules"
      class="login-form"
      auto-complete="on"
      label-position="left"
    >
      <!-- 头像区域 -->
      <div class="title-container">
        <h3 class="title">Toy Web</h3>
      </div>

      <el-form-item prop="username">
        <span class="svg-container">
          <i class="el-icon-user-solid"></i>
        </span>
        <el-input
          ref="username"
          v-model="loginForm.username"
          placeholder="Username"
          type="text"
          tabindex="1"
          auto-complete="off"
        />
      </el-form-item>

      <el-form-item prop="password">
        <span class="svg-container">
          <i class="el-icon-view"></i>
        </span>
        <el-input
          :key="passwordType"
          ref="password"
          v-model="loginForm.password"
          :type="passwordType"
          placeholder="Password"
          tabindex="2"
          auto-complete="off"
          show-password
          @keyup.enter.native="handleLogin"
        />
      </el-form-item>
      <div>
        <el-button
          type="primary"
          style="width: 100%; margin-bottom: 20px"
          @click.native.prevent="handleLogin"
          >登录</el-button
        >
      </div>
      <div class="tips">
        <span style="margin-right: 20px"
          >如果您还没有账号请先
          <span style="color: #409eff; cursor: pointer" @click="register"
            >注册</span
          ></span
        >
      </div>
    </el-form>
    <UserRegister v-if="userReg" @okayRegister="okayRegister" />
  </div>
</template>

<script>
// 引入去除空格工具
// import { validUsername } from "@/utils/validate";
import UserRegister from "./components/userRegister.vue";
export default {
  name: "toyLogin",
  components: {
    UserRegister,
  },
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!value) {
        callback(new Error("用户名不能为空！"));
      } else {
        callback();
      }
    };
    const validatePassword = (rule, value, callback) => {
      if (!value) {
        callback(new Error("密码不能为空"));
      } else {
        callback();
      }
    };
    return {
      loginForm: {
        username: "",
        password: null,
      },
      // 登录规则
      loginRules: {
        username: [
          { required: true, trigger: "blur", validator: validateUsername },
        ],
        password: [
          { required: true, trigger: "blur", validator: validatePassword },
        ],
      },
      loading: false,
      passwordType: "password",
      redirect: undefined,
      userReg: false,
      registerObject: {},
    };
  },
  watch: {
    // $route: {
    //   handler: function (route) {
    //     this.redirect = route.query && route.query.redirect;
    //   },
    //   immediate: true,
    // },
  },
  methods: {
    // 登录业务
    handleLogin() {
      this.$refs.loginForm.validate((valid) => {
        // 如果符合验证规则
        if (valid) {
          this.loading = true;
          if (
            this.loginForm.username === this.registerObject.username &&
            this.loginForm.password === this.registerObject.password
          ) {
            setTimeout(() => {
              this.loading = false;
              this.$message({
                message: `欢迎你${this.loginForm.username}`,
                type: "success",
              });
              this.$router.push({
                name: "toyMain",
                query: this.registerObject,
              });
            }, 1500);
          } else if (
            this.loginForm.username === "root" &&
            this.loginForm.password === "123"
          ) {
            setTimeout(() => {
              this.loading = false;
              this.$message({
                message: `欢迎你${this.loginForm.username}`,
                type: "success",
              });
              this.$router.push({
                name: "toyMain",
              });
            }, 1500);
          } else {
            setTimeout(() => {
              this.loading = false;
              this.$message.error("用户名或密码错误");
            }, 1500);
          }
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    // 注册业务
    register() {
      console.log("123");
      // this.$router.push({ name: "register" });
      this.userReg = true;
    },
    okayRegister(data) {
      this.userReg = false;
      this.registerObject = data;
    },
  },
};
</script>

<style lang="scss">
$bg: #283443;
$light_gray: #fff;
$cursor: #fff;
@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}
.login-container {
  height: 100vh;
  overflow: auto;
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;

    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      caret-color: $cursor;

      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}
</style>

<style lang="scss" scoped>
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;

.login-container {
  min-height: 100%;
  width: 100%;
  overflow: hidden;
  background: url("../assets/login.jpg");
  background-size: 100% 100%;
  // 头像

  .login-form {
    position: relative;
    width: 420px;
    max-width: 100%;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;
  }

  .tips {
    font-size: 18px;
    text-align: center;
    color: #000;
    margin-bottom: 10px;
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;

    .title {
      font-size: 30px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: 500;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
}
</style>
