<template>
  <div class="nav-container page-component">

    <!-- Left Navigation -->
    <div class="nav left-nav">
      <!-- Navigation Items -->
      <div class="nav-item selected">
        <span class="v-link selected dark" onclick="javascript:window.location='/user'">实名认证</span>
      </div>
      <div class="nav-item">
        <span class="v-link selected dark" onclick="javascript:window.location='/order'">挂号订单</span>
      </div>
      <div class="nav-item">
        <span class="v-link clickable dark" onclick="javascript:window.location='/patient'">就诊人管理</span>
      </div>
      <div class="nav-item selected">
        <span class="v-link clickable dark" onclick="javascript:window.location='/user'">修改账号信息</span>
      </div>
      <div class="nav-item">
        <span class="v-link clickable dark">意见反馈</span>
      </div>
    </div>

    <!-- Right Content -->
    <div class="page-container">
      <div>
        <div class="title">修改账号信息</div>
        <div class="form-wrapper" v-if="userInfo.authStatus == 0">
          <el-form :model="accountInfo" label-width="110px" label-position="left">
            <el-form-item prop="username" label="用户名：">
              <el-input v-model="accountInfo.username" placeholder="请输入您的用户名" class="input v-input"/>
            </el-form-item>
            <el-form-item prop="email" label="邮箱：">
              <el-input v-model="accountInfo.email" placeholder="请输入您的邮箱地址" class="input v-input"/>
            </el-form-item>
            <el-form-item prop="phone" label="手机号码：">
              <el-input v-model="accountInfo.phone" placeholder="请输入您的手机号码" class="input v-input"/>
            </el-form-item>
            <el-form-item prop="password" label="密码：">
              <el-input type="password" v-model="accountInfo.password" placeholder="请输入您的密码" class="input v-input"/>
            </el-form-item>
          </el-form>

          <div class="bottom-wrapper">
            <div class="button-wrapper">
              <div class="v-button" @click="updateAccountInfo()">{{ submitBtn }}</div>
            </div>
          </div>
        </div>

        <div class="context-container" v-if="userInfo.authStatus != 0">
          <div>
            <el-form :model="userInfo" label-width="110px" label-position="right">
              <el-form-item prop="username" label="用户名：">
                {{ userInfo.username }}
              </el-form-item>
              <el-form-item prop="email" label="邮箱：">
                {{ userInfo.email }}
              </el-form-item>
              <el-form-item prop="phone" label="手机号码：">
                {{ userInfo.phone }}
              </el-form-item>
            </el-form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import '~/assets/css/hospital_personal.css'
import '~/assets/css/hospital.css'
import '~/assets/css/personal.css'

import userInfoApi from '@/api/user/userInfo'

const defaultForm = {
  username: '',
  email: '',
  phone: '',
  password: ''
}

export default {
  data() {
    return {
      accountInfo: defaultForm,
      userInfo: {},
      submitBtn: '提交'
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      this.getUserInfo()
    },
    getUserInfo() {
      userInfoApi.getUserInfo().then(response => {
        this.userInfo = response.data
        this.accountInfo = {...this.userInfo} // Pre-fill the form with current info
      })
    },
    updateAccountInfo() {
      if (this.submitBtn === '正在提交...') {
        this.$message.info('重复提交')
        return
      }
      this.submitBtn = '正在提交...'
      userInfoApi.updateAccountInfo(this.accountInfo).then(() => {
        this.$message.success("更新成功")
        window.location.reload()
      }).catch(() => {
        this.submitBtn = '提交'
      })
    }
  }
}
</script>

<style scoped>
/* Custom styles for the page */
.page-container .title {
  letter-spacing: 1px;
  font-weight: 700;
  color: #333;
  font-size: 16px;
  margin-top: 0;
  margin-bottom: 20px;
}
.form-normal {
  height: 40px;
}
.bottom-wrapper {
  width: 100%;
  padding: 0;
  margin-top: 0;
}
</style>
