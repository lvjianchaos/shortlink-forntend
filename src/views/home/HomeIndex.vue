<template>
  <div class="common-layout">
    <el-main>
      <el-header height="54px" class="header-wrap">
        <div class="header">
          <button @click="toMySpace" class="logo" type="button">Chaos🔗短链接</button>
          <div class="header-right">
            <a
              class="github-link"
              target="_blank"
              href="https://github.com/lvjianchaos"
              rel="noopener noreferrer"
              >            
              <el-icon><Link /></el-icon>
              <span>Github</span>   
            </a>
            <el-dropdown>
              <div class="user-entry">
                <el-icon><UserFilled /></el-icon>
                <span class="name-span">{{ username }}</span>
              </div>
              <template #dropdown>
                <el-dropdown-menu>
                  <el-dropdown-item @click="toMine">个人信息</el-dropdown-item>
                  <el-dropdown-item divided @click="logout">退出</el-dropdown-item>
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </div>
        </div>
      </el-header>
      <el-main class="main-wrap">
        <div class="content-box">
          <div class="content-space">
            <RouterView />
          </div>
        </div>
      </el-main>
        <el-aside width="180px">
          <el-menu
            active-text-color="#073372"
            background-color="#0e5782"
            class="el-menu-vertical-demo"
            :default-active="getLasteRoute(route.path)"
            text-color="#fff"
            @select="handleSelect"
          >
            <template v-for="item in menuInfos" :key="item.name">
              <el-menu-item :index="item.path">
                <el-icon><icon-menu /></el-icon>
                <span>{{ item.name }}</span>
              </el-menu-item>
            </template>
          </el-menu></el-aside
        >
    </el-main>
  </div>
</template>

<script setup>
import { ref, getCurrentInstance, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { removeKey, removeUsername, getToken, getUsername } from '@/core/auth.js'
import { ElMessage } from 'element-plus'
import { Link, UserFilled } from '@element-plus/icons-vue'

const { proxy } = getCurrentInstance()
const API = proxy.$API
const router = useRouter()

const toMine = () => {
  router.push('/home' + '/account')
}

// 登出
const logout = async () => {
  const token = getToken()
  const username = getUsername()

  try {
    await API.user.logout({ token, username })
  } catch (error) {
    // 忽略登出接口异常，保证前端状态可正常清理
  }

  // 删除cookies中的token和username
  removeUsername()
  removeKey()
  localStorage.removeItem('token')
  localStorage.removeItem('username')
  router.push('/login')
  ElMessage.success('成功退出！')
}
// 点击左上方的图片跳转到我的空间
const toMySpace = () => {
  router.push('/home' + '/space')
}

const username = ref('User')

onMounted(async () => {
  const actualUsername = getUsername() || 'User'
  await API.user.queryUserInfo(actualUsername)
  username.value = truncateText(actualUsername, 8)
})

// 辅助函数，用于截断文本
const truncateText = (text, maxLength) => {
  return text.length > maxLength ? text.slice(0, maxLength) + '...' : text
}
</script>

<style lang="scss" scoped>
.common-layout {
  min-height: 100vh;
  background: linear-gradient(145deg, #eef2f8 0%, #e9eef6 50%, #edf1f8 100%);
}

.el-container {
  height: 100vh;
  background: transparent;
}

.header-wrap {
  padding: 8px 14px;
  background: transparent;
}

.header {
  height: 38px;
  padding: 0 14px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-radius: 18px;
  background: linear-gradient(145deg, #eef3fa 0%, #e7edf6 100%);
  box-shadow:
    8px 8px 18px rgba(157, 172, 196, 0.35),
    -8px -8px 18px rgba(255, 255, 255, 0.92),
    inset 1px 1px 0 rgba(255, 255, 255, 0.85),
    inset -1px -1px 0 rgba(200, 213, 231, 0.5);
}

.header-right {
  display: flex;
  align-items: center;
  gap: 8px;
}

.main-wrap {
  padding: 0;
  background: transparent;
}

.content-box {
  height: calc(100vh - 54px);
  padding: 16px;
  background: transparent;
}

.content-space {
  height: 100%;
  border-radius: 20px;
  background: linear-gradient(145deg, #edf2f9 0%, #e8eef7 100%);
  box-shadow:
    12px 12px 24px rgba(154, 171, 197, 0.36),
    -12px -12px 24px rgba(255, 255, 255, 0.95),
    inset 1px 1px 0 rgba(255, 255, 255, 0.8);
  overflow: hidden;
}

:deep(.content-space > *) {
  height: 100%;
}

:deep(.el-tooltip__trigger:focus-visible) {
  outline: unset;
}

:deep(.el-dropdown-menu) {
  border: 0;
  border-radius: 14px;
  background: linear-gradient(145deg, #edf2f9 0%, #e9eef7 100%);
  box-shadow:
    10px 10px 20px rgba(162, 179, 203, 0.3),
    -8px -8px 16px rgba(255, 255, 255, 0.9);
}

:deep(.el-dropdown-menu__item) {
  border-radius: 10px;
  margin: 4px 6px;
}

:deep(.el-dropdown-menu__item:hover) {
  background: rgba(255, 255, 255, 0.68);
}

.logo {
  border: 0;
  background: transparent;
  padding: 6px 12px;
  font-size: 15px;
  font-weight: 600;
  color: #2d405f;
  font-family: Helvetica, Tahoma, Arial, 'PingFang SC', 'Hiragino Sans GB', 'Heiti SC',
    'Microsoft YaHei', 'WenQuanYi Micro Hei';
  cursor: pointer;
  border-radius: 999px;
  box-shadow:
    5px 5px 10px rgba(162, 178, 202, 0.28),
    -5px -5px 10px rgba(255, 255, 255, 0.9),
    inset 1px 1px 0 rgba(255, 255, 255, 0.78);
  transition: all 0.18s cubic-bezier(0.4, 0, 0.2, 1);
}

.logo:hover {
  color: #1f3557;
  transform: translateY(-1px);
  box-shadow:
    8px 8px 14px rgba(155, 171, 194, 0.34),
    -8px -8px 14px rgba(255, 255, 255, 0.95),
    inset 1px 1px 0 rgba(255, 255, 255, 0.88);
}

.logo:active {
  transform: translateY(0);
  box-shadow:
    inset 5px 5px 10px rgba(163, 178, 201, 0.35),
    inset -5px -5px 10px rgba(255, 255, 255, 0.88);
}

.github-link {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: #2c4f82;
  margin-right: 4px;
  padding: 7px 12px;
  border-radius: 999px;
  font-size: 13px;
  font-weight: 600;
  letter-spacing: 0.2px;
  font-family: 'Helvetica Neue', Helvetica, STHeiTi, Arial, sans-serif;
  text-decoration: none;
  border: 1px solid rgba(102, 132, 178, 0.25);
  background: linear-gradient(145deg, #e1ebf9 0%, #d8e5f6 100%);
  box-shadow:
    6px 6px 12px rgba(148, 169, 200, 0.34),
    -6px -6px 12px rgba(247, 251, 255, 0.9),
    inset 1px 1px 0 rgba(255, 255, 255, 0.82);
  transition: all 0.18s cubic-bezier(0.4, 0, 0.2, 1);
}

.github-link .el-icon {
  font-size: 14px;
  flex-shrink: 0;
}

.github-link:hover {
  color: #1f4376;
  transform: translateY(-1px);
  border-color: rgba(98, 130, 177, 0.45);
  background: linear-gradient(145deg, #e6effd 0%, #dce9fa 100%);
  box-shadow:
    9px 9px 16px rgba(147, 167, 197, 0.36),
    -9px -9px 16px rgba(255, 255, 255, 0.93);
}

.github-link:active {
  transform: translateY(0);
  color: #2e507f;
  box-shadow:
    inset 4px 4px 9px rgba(141, 160, 190, 0.38),
    inset -4px -4px 9px rgba(247, 251, 255, 0.88);
}

.user-entry {
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 6px;
  min-height: 32px;
  max-width: 160px;
  padding: 0 12px;
  border-radius: 999px;
  border: 1px solid rgba(255, 255, 255, 0.62);
  background: linear-gradient(145deg, #eef4fb 0%, #e9f0fa 100%);
  box-shadow:
    5px 5px 11px rgba(163, 180, 202, 0.25),
    -5px -5px 11px rgba(255, 255, 255, 0.95),
    inset 1px 1px 0 rgba(255, 255, 255, 0.8);
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.user-entry .el-icon {
  font-size: 13px;
  color: #6a81a6;
  flex-shrink: 0;
}

.user-entry:hover {
  transform: translateY(-1px);
  background: linear-gradient(145deg, #f1f6fc 0%, #ecf2fb 100%);
  box-shadow:
    8px 8px 14px rgba(161, 177, 198, 0.28),
    -8px -8px 14px rgba(255, 255, 255, 0.96);
}

.user-entry:active {
  transform: translateY(0);
  box-shadow:
    inset 4px 4px 10px rgba(161, 176, 200, 0.36),
    inset -4px -4px 10px rgba(255, 255, 255, 0.9);
}

.name-span {
  display: block;
  max-width: 120px;
  color: #5c7398;
  font-size: 12px;
  font-weight: 600;
  font-family: 'Helvetica Neue', Helvetica, STHeiTi, Arial, sans-serif;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

@media (max-width: 768px) {
  .header {
    padding: 0 10px;
  }

  .github-link {
    display: none;
  }

  .content-box {
    padding: 10px;
  }

  .content-space {
    border-radius: 16px;
  }
}
</style>
