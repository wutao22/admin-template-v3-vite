<template>
  <div class="login-container">
    <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on"
      label-position="left">
      <div class="title-container">
        <h3 class="title">登录</h3>
      </div>
      <el-form-item prop="username" class="margin-tb">
        <span class="svg-container">
          <el-icon>
            <component :is="'User'"></component>
          </el-icon>
        </span>
        <input ref="username" v-model="state.loginForm.username" placeholder="请输入用户名" name="username" type="text"
          tabindex="1" auto-complete="on" />
      </el-form-item>

      <el-form-item prop="password" class ="pwd">
        <span class="svg-container">
          <el-icon>
            <component :is="'Lock'"></component>
          </el-icon>
          <!-- <svg-icon icon-class="password" /> -->
        </span>
        <input :key="passwordType" ref="password" v-model="state.loginForm.password" :type="passwordType"
          placeholder="请输入用户密码" name="password" tabindex="2" auto-complete="on" @keyup.enter.native="handleLogin" />
        <span class="show-pwd" @click="showPwd">
          <el-icon>
            <component :is="passwordType === 'password' ? 'View' : 'Hide'"></component>
          </el-icon>
        </span>
      </el-form-item>

      <el-button :loading="loading" type="primary" style="width: 100%; margin: 30px 0; padding: 20px;"
        @click.native.prevent="handleLogin">登录</el-button>
    </el-form>
  </div>
</template>

<script setup lang="ts">
import axios from "axios";
import {
  onMounted,
  reactive,
  ref,
  toRef,
  toRefs,
  watch,
  nextTick,
  getCurrentInstance,
} from "vue";
import { useRoute } from "vue-router";
const { proxy }: any = getCurrentInstance();
const route = useRoute();
// const loginForm =
const state = reactive({
  loginForm: {
    username: "",
    password: "",
  },
  loginRules: {
    username: [{ required: true, trigger: "blur" }],
    password: [{ required: true, trigger: "blur" }],
  },
  loading: false,
  passwordType: "password",
});
const { loading, loginForm, loginRules, passwordType } = toRefs(state);

function showPwd() {
  if ((state.passwordType = "password")) {
    state.passwordType = "text";
  } else {
    state.passwordType = "password";
  }
}
// 点击登录
function handleLogin() {
  state.loading = true;
  proxy
    .post("/user/login", {
      username: state.loginForm.username,
      password: state.loginForm.password,
    })
    .then((res: any) => {
      console.log(res);
    });
}
</script>


<style lang="scss">
$bg: #283443;
$light_gray: #fff;
$cursor: #fff;

/* reset element-ui css */
.login-container {
  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }

    .set-language {
      color: #fff;
      position: absolute;
      top: 3px;
      font-size: 18px;
      right: 0px;
      cursor: pointer;
    }
  }

  input {
    display: inline-block;
    height: 46px;
    background: transparent;
    border: none;
    color: white;
    outline: none;
    text-align: left;
    width: 85%;

    .el-input__wrapper {
      padding: 0;
      background: transparent;
      box-shadow: none;

      .el-input__inner {
        background: transparent;
        border: 0px;
        -webkit-appearance: none;
        border-radius: 0px;
        color: $light_gray;
        height: 46px;
        caret-color: $cursor;

        &:-webkit-autofill {
          box-shadow: 0 0 0px 1000px $bg inset !important;
          -webkit-text-fill-color: $cursor !important;
        }
      }
    }
  }

  .el-input__inner {
    &:hover {
      border-color: var(--el-input-hover-border, var(--el-border-color-hover));
      box-shadow: none;
    }

    box-shadow: none;
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }

  .copyright {
    width: 100%;
    position: absolute;
    bottom: 0;
    font-size: 12px;
    text-align: center;
    color: #cccccc;
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
  background-color: $bg;
  overflow: hidden;

  .login-form {
    position: relative;
    width: 520px;
    max-width: 100%;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;
  }

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 5px 10px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
  }

  .pwd {
    position: relative;
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 13px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }

  .captcha {
    position: absolute;
    right: 0;
    top: 0;

    img {
      height: 42px;
      cursor: pointer;
      vertical-align: middle;
    }
  }
}

.thirdparty-button {
  position: absolute;
  right: 40px;
  bottom: 6px;
}

@media only screen and (max-width: 470px) {
  .thirdparty-button {
    display: none;
  }
}
</style>
