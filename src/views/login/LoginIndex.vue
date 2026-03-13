<template>
  <div class="login-page">
    <div class="login-box">
      <!-- 登录 -->
      <div class="logon" :class="{ hidden: !isLogin }">
        <h2>用户登录</h2>
        <el-form ref="loginFormRef1" :model="loginForm" label-width="50px" :rules="loginFormRule">
          <div class="form-container1">
            <el-form-item prop="phone">
              <el-input v-model="loginForm.username" placeholder="请输入用户名" maxlength="11" show-word-limit clearable>
                <template v-slot:prepend> 用户名 </template>
              </el-input>
            </el-form-item>

            <el-form-item prop="password">
              <el-input v-model="loginForm.password" type="password" clearable placeholder="请输入密码" show-password
                class="password-input">
                <template v-slot:prepend> 密<span class="second-font">码</span> </template>
              </el-input>
            </el-form-item>
          </div>
          <div class="btn-gourp">
            <div>
              <el-checkbox class="remeber-password" v-model="checked">记住密码</el-checkbox>
            </div>
            <div>
              <el-button :loading="loginLoading" :disabled="loginLoading" @keyup.enter="login" type="primary" plain
                @click="login(loginFormRef1)">登录</el-button>
            </div>
          </div>
        </el-form>
      </div>
      <!-- 注册 -->
      <div class="register" :class="{ hidden: isLogin }">
        <h2>用户注册</h2>
        <el-form ref="loginFormRef2" :model="addForm" label-width="50px" class="form-container" width="width"
          :rules="addFormRule">
          <el-form-item prop="username">
            <el-input v-model="addForm.username" placeholder="请输入用户名" maxlength="11" show-word-limit clearable>
              <template v-slot:prepend> 用户名 </template>
            </el-input>
          </el-form-item>
          <el-form-item prop="mail">
            <el-input v-model="addForm.mail" placeholder="请输入邮箱" show-word-limit clearable>
              <template v-slot:prepend> 邮<span class="second-font">箱</span> </template>
            </el-input>
          </el-form-item>
          <el-form-item prop="phone">
            <el-input v-model="addForm.phone" placeholder="请输入手机号" show-word-limit clearable>
              <template v-slot:prepend> 手机号 </template>
            </el-input>
          </el-form-item>
          <el-form-item prop="realName">
            <el-input v-model="addForm.realName" placeholder="请输入姓名" show-word-limit clearable>
              <template v-slot:prepend> 姓<span class="second-font">名</span> </template>
            </el-input>
          </el-form-item>

          <el-form-item prop="password">
            <el-input v-model="addForm.password" type="password" clearable placeholder="请输入密码" show-password>
              <template v-slot:prepend> 密<span class="second-font">码</span> </template>
            </el-input>
          </el-form-item>
          <!-- 验证码 -->
          <!-- <el-form-item prop="vertify_code">
            <el-input
              v-model="loginForm.vertify_code"
              placeholder="验证码"
              prefix-icon="el-icon-key"
              clearable
            >
              <template v-slot:append>
                <div class="login-code" @click="refreshCode" title="看不清？点击切换">
                  <vertify-code :identifyCode="loginIdentifyCode"></vertify-code>
                </div>
              </template>
            </el-input>
          </el-form-item> -->
          <div class="btn-gourp">
            <div></div>
            <div>
              <el-button :loading="registerLoading" :disabled="registerLoading" @keyup.enter="login" type="primary" plain
                @click="addUser(loginFormRef2)">注册</el-button>
            </div>
          </div>
        </el-form>
      </div>
      <!-- 左右移动的切换按钮 -->
      <div class="move" :class="{ 'move-left': !isLogin }">
        <span class="move-title">{{
          !isLogin ? '已有账号？' : '还没有账号？'
        }}</span>
        <span class="move-subtitle">{{
          !isLogin ? '欢迎登录账号！' : '欢迎注册账号！'
        }}</span>
        <el-button class="switch-btn" @click="changeLogin">{{
          !isLogin ? '去登录' : '去注册'
        }}</el-button>
      </div>
    </div>
    <div ref="vantaRef" class="vanta"></div>
  </div>
</template>

<script setup>
import { setToken, setUsername, getUsername } from '@/core/auth.js'
import { ref, reactive, onMounted, onBeforeUnmount, getCurrentInstance } from 'vue'
import { useRouter } from 'vue-router'
import { ElMessage } from 'element-plus'
import * as THREE from 'three'
import WAVES from 'vanta/src/vanta.waves'
const { proxy } = getCurrentInstance()
const API = proxy.$API
const loginFormRef1 = ref()
const loginFormRef2 = ref()
const router = useRouter()
const loginForm = reactive({
  username: 'admin',
  password: 'admin123456',
})
const addForm = reactive({
  username: '',
  password: '',
  realName: '',
  phone: '',
  mail: ''
})

const addFormRule = reactive({
  phone: [
    { required: true, message: '请输入手机号', trigger: 'blur' },
    {
      pattern: /^1[3|5|7|8|9]\d{9}$/,
      message: '请输入正确的手机号',
      trigger: 'blur'
    },
    { min: 11, max: 11, message: '手机号必须是11位', trigger: 'blur' }
  ],
  username: [{ required: true, message: '请输入您的真实姓名', trigger: 'blur' }],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    { min: 8, max: 15, message: '密码长度请在八位以上', trigger: 'blur' }
  ],
  mail: [
    { required: true, message: '请输入邮箱', trigger: 'blur' },
    {
      pattern: /^([a-zA-Z]|[0-9])(\w|-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/,
      message: '请输入正确的邮箱号',
      trigger: 'blur'
    }
  ],
  realNamee: [
    { required: true, message: '请输姓名', trigger: 'blur' },
  ]
})
const loginFormRule = reactive({
  username: [{ required: true, message: '请输入您的真实姓名', trigger: 'blur' }],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur' },
    { min: 8, max: 15, message: '密码长度请在八位以上', trigger: 'blur' }
  ],
})
// 注册
const addUser = async (formEl) => {
  if (!formEl || registerLoading.value) return
  registerLoading.value = true
  try {
    const valid = await formEl.validate().then(() => true).catch(() => false)
    if (!valid) return false

    // 检测用户名是否已经存在
    const res1 = await API.user.hasUsername({ username: addForm.username })
    if (res1.data.success !== false) {
      // 注册
      const res2 = await API.user.addUser(addForm)
      if (res2.data.success === false) {
        ElMessage.warning(res2.data.message)
      } else {
        const res3 = await API.user.login({ username: addForm.username, password: addForm.password })
        const token = res3?.data?.data?.token
        // 将username和token保存到cookies中和localStorage中
        if (token) {
          setToken(token)
          setUsername(addForm.username)
          localStorage.setItem('token', token)
          localStorage.setItem('username', addForm.username)
        }
        ElMessage.success('注册登录成功！')
        router.push('/home')
      }
    } else {
      ElMessage.warning('用户名已存在！')
    }
  } finally {
    registerLoading.value = false
  }
}
// 登录
const login = async (formEl) => {
  if (!formEl || loginLoading.value) return
  loginLoading.value = true
  try {
    const valid = await formEl.validate().then(() => true).catch(() => false)
    if (!valid) return false

    const res1 = await API.user.login(loginForm)
    if (res1.data.code === '0') {
      const token = res1?.data?.data?.token
      // 将username和token保存到cookies中和localStorage中
      if (token) {
        setToken(token)
        setUsername(loginForm.username)
        localStorage.setItem('token', token)
        localStorage.setItem('username', loginForm.username)
      }
      ElMessage.success('登录成功！')
      router.push('/home')
    } else if (res1.data.message === '用户已登录') {
      // 如果已经登录了，判断一下浏览器保存的登录信息是不是再次登录的信息，如果是就正常登录
      const cookiesUsername = getUsername()
      if (cookiesUsername === loginForm.username) {
        ElMessage.success('登录成功！')
        router.push('/home')
      } else {
        ElMessage.warning('用户已在别处登录，请勿重复登录！')
      }
    } else if (res1.data.message === '用户不存在') {
      ElMessage.error('请输入正确的账号密码!')
    }
  } finally {
    loginLoading.value = false
  }
}

const loginLoading = ref(false)
const registerLoading = ref(false)
// 是否记住密码
const checked = ref(true)
const vantaRef = ref()
// 动态背景
let vantaEffect = null
onMounted(() => {
  vantaEffect = WAVES({
    el: vantaRef.value,
    THREE: THREE,
    mouseControls: true,
    touchControls: true,
    gyroControls: false,
    minHeight: 200.0,
    minWidth: 200.0,
    scale: 1.0,
    scaleMobile: 1.0
  })
})
onBeforeUnmount(() => {
  if (vantaEffect) {
    vantaEffect.destroy()
  }
})
// 展示登录还是展示注册
const isLogin = ref(true)
const changeLogin = () => {
  let domain = window.location.host
  if (domain === 'shortlink.magestack.cn' || domain === 'shortlink.nageoffer.com') {
    ElMessage.warning('演示环境暂不支持注册')
    return
  }
  isLogin.value = !isLogin.value
}
</script>

<style lang="less" scoped>
.login-page {
  --bg-1: #eef2f8;
  --bg-2: #e9eef6;
  --bg-3: #edf1f8;
  --surface: linear-gradient(145deg, #edf2f9 0%, #e8eef7 100%);
  --ink: #334f77;
  --ink-soft: #6e85a7;
  --primary: #4f79b8;
  --primary-deep: #426a9f;
}

.login-box {
  overflow: hidden;
  display: flex;
  justify-content: space-between;
  border-radius: 26px;
  padding: 0 28px;
  width: 760px;
  position: absolute;
  z-index: 2;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-sizing: border-box;
  background: var(--surface);
  box-shadow:
    14px 14px 28px rgba(152, 169, 194, 0.34),
    -14px -14px 28px rgba(255, 255, 255, 0.94),
    inset 1px 1px 0 rgba(255, 255, 255, 0.85);
  animation: hideIndex 0.5s;

  h2 {
    font-size: 28px;
    font-family:
      PingFangSC-Semibold,
      PingFang SC;
    font-weight: 600;
    color: var(--ink);
    width: 100%;
    text-align: center;
    padding: 30px 0 55px 0;
  }

  .el-form-item {
    margin-bottom: 20px;
  }

  .btn-gourp {
    margin-top: 18px;
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;

    .el-button {
      width: 106px;
      border-radius: 999px;
      border: 0;
      color: #fff;
      background: linear-gradient(145deg, var(--primary) 0%, var(--primary-deep) 100%);
      box-shadow:
        6px 6px 12px rgba(137, 157, 186, 0.34),
        -6px -6px 12px rgba(255, 255, 255, 0.86);
      transition: all 0.18s cubic-bezier(0.4, 0, 0.2, 1);
    }

    .el-button:hover {
      transform: translateY(-1px);
    }

    .el-button:active {
      transform: translateY(0);
      box-shadow:
        inset 4px 4px 8px rgba(118, 137, 165, 0.36),
        inset -4px -4px 8px rgba(255, 255, 255, 0.8);
    }

    .remeber-password {
      left: 0;
      line-height: 0.5rem;
      color: var(--ink-soft);
      margin: 0;
    }
  }

  .el-checkbox {
    width: 100%;
    text-align: center;
    margin-top: 1rem;
  }
}

:deep(.el-form-item__content) {
  margin-left: 0 !important;
}

:deep(.el-input__wrapper),
:deep(.el-input-group__prepend) {
  background: linear-gradient(145deg, #eef3fa 0%, #e7edf6 100%) !important;
  border: 0 !important;
  box-shadow:
    inset 3px 3px 7px rgba(168, 183, 205, 0.32),
    inset -3px -3px 7px rgba(255, 255, 255, 0.88) !important;
}

:deep(.el-input-group__prepend) {
  color: #58729a;
}

:deep(.el-input__inner) {
  color: #304866;
}

@keyframes hideIndex {

  // <!--具体细节自己可以调整-->
  0% {
    opacity: 0;
    transform: translate(7.3125rem, -50%);
  }

  100% {
    opacity: 1;
    transform: translate(-50%, -50%);
  }
}

.login-page {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: linear-gradient(145deg, var(--bg-1) 0%, var(--bg-2) 50%, var(--bg-3) 100%);
}

.vanta {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  z-index: 0;
}

.logon {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  width: 56%;
  min-height: 500px;
}

.register {
  width: 56%;
  min-height: 500px;
}

.password-input {
  margin-top: 20px;
}

.hidden {
  animation: hidden 1s;
  animation-fill-mode: forwards; // 保持最后的状态
}

@keyframes hidden {

  // <!--具体细节自己可以调整-->
  0% {
    opacity: 1;
  }

  70% {
    opacity: 0;
  }

  100% {
    opacity: 0;
  }
}

.move {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  height: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 40%;
  transition: transform 0.5s ease, border-radius 0.35s ease;
  align-items: center;
  border-radius: 0 26px 26px 0;
  margin: 0;
  background: linear-gradient(145deg, #5f86bf 0%, #466ea8 100%);
  box-shadow:
    8px 8px 16px rgba(120, 142, 174, 0.26),
    inset 1px 1px 0 rgba(255, 255, 255, 0.25);
}

.move-left {
  transform: translateX(-150%);
  border-radius: 26px 0 0 26px;
}

.move-title {
  font-size: 18px;
  margin-bottom: 24px;
  color: rgb(231, 240, 252);
}

.move-subtitle {
  font-size: 16px;
  color: rgb(231, 240, 252);
}

.switch-btn {
  width: 108px;
  margin-top: 30px;
  border-radius: 999px;
  border: 1px solid rgba(255, 255, 255, 0.45);
  background: rgba(255, 255, 255, 0.12);
  color: #eef5ff;
  box-shadow:
    4px 4px 10px rgba(56, 86, 132, 0.36),
    -4px -4px 10px rgba(107, 144, 194, 0.45);
}

.switch-btn:hover {
  transform: translateY(-1px);
}

.switch-btn:active {
  transform: translateY(0);
  box-shadow:
    inset 4px 4px 8px rgba(54, 80, 121, 0.36),
    inset -4px -4px 8px rgba(117, 151, 198, 0.38);
}

.title {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 15%;
  z-index: 999;
  font-size: 40px;
  color: #fff;
  font-weight: bolder;
}

:deep(.el-input__suffix-inner) {
  width: 60px;
}

.form-container1 {
  transform: none;
  margin-top: 8px;
}

.second-font {
  margin-left: 13px;
}

@media (max-width: 900px) {
  .login-box {
    width: 92vw;
    max-width: 760px;
    padding: 0 18px;
  }

  .move {
    width: 36%;
  }
}

@media (max-width: 760px) {
  .login-box {
    flex-direction: column;
    padding: 16px;
    height: auto;
  }

  .logon,
  .register {
    width: 100%;
    min-height: unset;
  }

  .move {
    position: relative;
    width: 100%;
    margin: 8px 0 0;
    transform: none !important;
    padding: 20px 0;
    border-radius: 16px;
  }

  .form-container1 {
    transform: none;
  }
}
</style>
