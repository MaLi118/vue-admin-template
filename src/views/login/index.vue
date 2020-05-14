<template>
  <div class="login-container">
    <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on" label-position="left">

      <div class="top" />

      <div class="title-container">
        <span class="title-icon" />
        <span class="title">昭阳区干部管理系统</span>
      </div>

      <el-form-item prop="username" class="admin">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          ref="username"
          v-model="loginForm.username"
          placeholder="用户名"
          name="username"
          type="text"
          tabindex="1"
          auto-complete="on"
        />
      </el-form-item>

      <el-form-item prop="password">
        <span class="svg-container">
          <svg-icon icon-class="password" />
        </span>
        <el-input
          :key="passwordType"
          ref="password"
          v-model="loginForm.password"
          :type="passwordType"
          placeholder="密码"
          name="password"
          tabindex="2"
          auto-complete="on"
          @keyup.enter.native="handleLogin"
        />
        <span class="show-pwd" @click="showPwd">
          <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
        </span>
      </el-form-item>

      <el-button :loading="loading" type="primary" @click.native.prevent="handleLogin">登录</el-button>

      <div class="tips" />
    </el-form>
    <div class="bottom">
      昭通亮风台信息科技有限公司提供技术支持
    </div>
  </div>
</template>

<script>
import { validUsername } from '@/utils/validate'

export default {
  name: 'Login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error('请输入正确的用户名 !'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('密码不能少于6位 !'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: 'admin',
        password: '111111'
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm).then(() => {
            this.$router.push({ path: this.redirect || '/' })
            this.loading = false
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg:#03070c;
$light_gray:#000;
$cursor: #000;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}

/* reset element-ui css */
.login-container {
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
    border-bottom: 1px solid #d60c0129;
    background: #ffffff;
    color: #454545;
    width: 402px;
    height: 60px;
    margin: 0 24px;

    .el-form-item__content{
      line-height: 80px;
    }

    .el-form-item__error{
      position: absolute;
      top: 70%;
      left: 0;
    }
  }

  .el-button{
    width: 402px;
    height: 50px;
    background:#d60c01;
    border-color: #d60c0129;
    border-radius:6px;
    margin: 72px 24px 68px;
  }

}
</style>

<style lang="scss" scoped>
$dark_gray:#f00;
$light_gray:#eee;

.login-container {
  position: relative;
  min-height: 100%;
  width: 100%;
  background: url(~@/assets/login_images/bg/login_bg.png) no-repeat center center;
  background-size: 100% 100%;
  overflow: hidden;

  .login-form {
    position: relative;
    width: 450px;
    max-width: 100%;
    margin: 181px auto 0;
    background:#ffffff;
    border:2px solid #d60c0129;
    border-radius:8px;
    overflow: hidden;

    .admin{
      margin-top: 35px;
    }

    .top{
      width: 450px;
      height:13px;
      padding-left: 0;
      margin: 0 auto;
      background:#d60c01;
      border-radius:8px 8px 0px 0px;
    }

    .title-container{
      position: relative;
      width: 347px;
      height: 49px;
      margin: 49px 0 0 47px;

      .title-icon{
        width: 49px;
        height: 49px;
        display: inline-block;
        background: url(~@/assets/login_images/logo.svg) no-repeat;
        margin-right: 28px;
        overflow: hidden;
      }

      .title{
        width: 262px;
        height: 26px;
        line-height: 26px;
        margin: 11.5px 0px;
        display: inline-block;
        font: normal bold 22px '微软雅黑';
        color: #d60c01;
        overflow: hidden;
      }
    }
  }

  .tips {
    width: 450px;
    height: 87px;
    display: inline-block;
    background:url(~@/assets/login_images/login.jpg) no-repeat;

  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: #d60c01;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
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

  .bottom{
    color: #ffffff;
    font: normal 300 14px '微软雅黑';
    position: absolute;
    bottom: 35px;
    width: 301px;
    height: 19px;
    text-align: center;
    letter-spacing: 1.8px;
    max-width: 100%;
    overflow: hidden;
    margin: 0 43%;
  }
}
</style>
