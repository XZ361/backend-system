<template>
  <div class="login-container">
    <el-form ref="formRef" :model="form" :rules="rules" class="login-form">
      <div class="title-container">
        <h3 class="title">用户登录</h3>
      </div>
      <el-form-item prop="username">
        <svg-icon icon="user" class="svg-container"></svg-icon>
        <el-input v-model="form.username" />
      </el-form-item>
      <el-form-item prop="password">
        <!-- <el-icon :size="20" class="svg-container">
          <Edit />
        </el-icon> -->
        <svg-icon icon="password" class="svg-container"></svg-icon>
        <el-input v-model="form.password" :type="passwordType" />
        <svg-icon
          :icon="passwordType === 'password' ? 'eye' : 'eye-open'"
          @click="changeType"
        ></svg-icon>
      </el-form-item>
      <el-button type="primary" class="login-button" @click="handleLogin"
        >登录</el-button
      >
    </el-form>
  </div>
</template>
<script setup>
import { ref } from 'vue'
import { login } from '@/api/login'
const form = ref({
  username: '',
  password: ''
})
const rules = ref({
  username: [
    {
      required: true,
      message: '请输入合法的用户名',
      trigger: 'change'
    }
  ],
  password: [
    {
      required: true,
      message: '请输入正确的密码',
      trigger: 'change'
    },
    { min: 5, max: 8, message: 'Length should be 3 to 5', trigger: 'blur' }
  ]
})
const formRef = ref(null)
const handleLogin = () => {
  formRef.value.validate(async (valid) => {
    if (valid) {
      const res = await login(form.value)
      console.log(res)
    } else {
      console.log('error submit!!')
      return false
    }
  })
}
const passwordType = ref('password')
const changeType = () => {
  if (passwordType.value === 'password') {
    passwordType.value = 'text'
  } else {
    passwordType.value = 'password'
  }
}
</script>
<style lang="scss" scoped>
// $bg: #2d3a4b;
.login-container {
  min-height: 100%;
  width: 100%;
  background: #2d3a4b;
  overflow: hidden;
  .login-form {
    position: relative;
    width: 520px;
    max-width: 100%;
    padding: 160px 36px 0;
    margin: 0 auto;
    overflow: hidden;

    :deep .el-form-item {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }
    :deep .el-input {
      display: inline-block;
      height: 47px;
      width: 85%;
      color: #2d3a4b;

      input {
        background: transparent;
        border: 0px;
        -webkit-appearance: none;
        border-radius: 0px;
        padding: 12px 5px 12px 15px;
        height: 47px;
        width: 100%;
      }
    }
    .login-button {
      width: 100%;
      box-sizing: border-box;
    }
  }
  .tips {
    font-size: 16px;
    line-height: 28px;
    color: #fff;
    margin-bottom: 10px;
    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }
  .svg-container {
    padding: 6px 5px 6px 15px;
    display: inline-block;
    vertical-align: middle;
    color: #889aa4;
  }
  .title-container {
    position: relative;
    .title {
      font-size: 26px;
      margin: 0 auto 40px auto;
      text-align: center;
      font-weight: bold;
      color: #eee;
    }
  }
}
</style>
