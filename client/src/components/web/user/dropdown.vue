<template>
  <el-popover placement="bottom-end" trigger="click" :width="300" popper-class="user-info-popover">
    <template #reference>
      <div class="user-info">
        <img class="user-info-avatar" :src="userAvatar" alt="">
        <span class="user-info-name">{{ userInfo.username }}</span>
        <span class="user-info-role">{{ roles[userInfo.role] }}</span>
        <span class="user-info-caret">
          <el-icon>
            <CaretBottom />
          </el-icon>
        </span>
      </div>
    </template>
    <div class="user-info__content">
      <div class="popover__header">
        <div class="header-left">
          <img class="user-avatar" :src="userAvatar" alt="">
          <div>
            <span class="user-name">{{ userInfo.username }}</span>
            <span class="user-role">{{ roles[userInfo.role] }}</span>
          </div>
        </div>
        <div class="header-right">
          <el-button type="primary" @click="logout">退出登录</el-button>
        </div>
      </div>
      <div class="popover__content">
        <div class="personal-center">
          <div class="center-item" @click="goProfile('base')">
            <el-icon>
              <Postcard />
            </el-icon>
            <span>个人资料</span>
          </div>
          <div class="center-item" @click="goProfile('security')">
            <el-icon>
              <Lock />
            </el-icon>
            <span>修改密码</span>
          </div>
          <!-- 账号绑定：binding  新消息通知：notification -->
        </div>
        <div class="entry-list">
          <div class="entry-item" v-for="(item, index) in entries" :key="index" @click="goView(item)">
            <el-icon><component :is="item.icon"></component></el-icon>
            <span>{{ item.label }}</span>
          </div>
        </div>
      </div>
    </div>
  </el-popover>
</template>

<script lang="ts" setup>
import { UserInter } from '@/typings/interface';
import { computed, Ref, ref } from 'vue';
import { useRouter } from 'vue-router';
interface AvatarInter {
  name: string
  suffix: string
  url?: string
}

const props = withDefaults(defineProps<{ userInfo: UserInter, roles: any }>(), {
  userInfo: () => ({} as UserInter),
  roles: () => ({
    1: '',
    2: '',
    10: '管理员'
  })
})

// 所有头像列表
const avatars: Ref<AvatarInter[]> = ref([

  { name: '星座_白羊座', suffix: 'png' },
  { name: '星座_金牛座', suffix: 'png' },
  { name: '星座_双子座', suffix: 'png' },
  { name: '星座_巨蟹座', suffix: 'png' },
  { name: '星座_狮子座', suffix: 'png' },
  { name: '星座_处女座', suffix: 'png' },
  { name: '星座_天秤座', suffix: 'png' },
  { name: '星座_天蝎座', suffix: 'png' },
  { name: '星座_射手座', suffix: 'png' },
  { name: '星座_摩羯座', suffix: 'png' },
  { name: '星座_水瓶座', suffix: 'png' },
  { name: '星座_双鱼座', suffix: 'png' },

  
  { name: '生肖_鼠', suffix: 'png' },
  { name: '生肖_牛', suffix: 'png' },
  { name: '生肖_虎', suffix: 'png' },
  { name: '生肖_兔', suffix: 'png' },
  { name: '生肖_龙', suffix: 'png' },
  { name: '生肖_蛇', suffix: 'png' },
  { name: '生肖_马', suffix: 'png' },
  { name: '生肖_羊', suffix: 'png' },
  { name: '生肖_猴', suffix: 'png' },
  { name: '生肖_鸡', suffix: 'png' },
  { name: '生肖_狗', suffix: 'png' },
  { name: '生肖_猪', suffix: 'png' },

  
  { name: '比赛_1', suffix: 'png' },
  { name: '比赛_2', suffix: 'png' },
  { name: '比赛_3', suffix: 'png' },
  { name: '比赛_4', suffix: 'png' },
  { name: '比赛_5', suffix: 'png' },
  { name: '比赛_6', suffix: 'png' },
  { name: '比赛_7', suffix: 'png' },
  { name: '比赛_8', suffix: 'png' },
  { name: '比赛_9', suffix: 'png' },
  { name: '比赛_10', suffix: 'png' },
  { name: '比赛_11', suffix: 'png' },
  { name: '比赛_12', suffix: 'png' },
  
  
  { name: '学生卡通_1', suffix: 'png' },
  { name: '学生卡通_2', suffix: 'png' },
  { name: '学生卡通_3', suffix: 'png' },
  { name: '学生卡通_4', suffix: 'png' },
  { name: '学生卡通_5', suffix: 'png' },
  { name: '学生卡通_6', suffix: 'png' },
  { name: '学生卡通_7', suffix: 'png' },
  { name: '学生卡通_8', suffix: 'png' },
  { name: '学生卡通_9', suffix: 'png' },
  { name: '学生卡通_10', suffix: 'png' },
  { name: '学生卡通_11', suffix: 'png' },
  { name: '学生卡通_12', suffix: 'png' },
  
  
  { name: '学生扁平_1', suffix: 'png' },
  { name: '学生扁平_2', suffix: 'png' },
  { name: '学生扁平_3', suffix: 'png' },
  { name: '学生扁平_4', suffix: 'png' },
  { name: '学生扁平_5', suffix: 'png' },
  { name: '学生扁平_6', suffix: 'png' },
  { name: '学生扁平_7', suffix: 'png' },
  { name: '学生扁平_8', suffix: 'png' },
  { name: '学生扁平_9', suffix: 'png' },
  { name: '学生扁平_10', suffix: 'png' },
  { name: '学生扁平_11', suffix: 'png' },
  { name: '学生扁平_12', suffix: 'png' },
  
  { name: '通用_1', suffix: 'png' },
  { name: '通用_2', suffix: 'png' },
  { name: '通用_3', suffix: 'png' },
  { name: '通用_4', suffix: 'png' },
  { name: '通用_5', suffix: 'png' },
  { name: '通用_6', suffix: 'png' },
  { name: '通用_7', suffix: 'png' },
  { name: '通用_8', suffix: 'png' },
  { name: '通用_9', suffix: 'png' },
  { name: '通用_10', suffix: 'png' },
  { name: '通用_11', suffix: 'png' },
  { name: '通用_12', suffix: 'png' },
])
avatars.value.forEach(item => {
  item.url = new URL(`../../../views/userinfo/images/${item.name}.${item.suffix}`, import.meta.url).href
})
const userAvatar = computed(() => {
  // props.userInfo.avatar
  return avatars.value.find(item => item.name === props.userInfo.avatar).url
})

const router = useRouter()

const entries = ref([
  { label: '使用分析', icon: 'DataAnalysis', route: 'Analysis' },
  { label: '使用习惯', icon: 'Setting', route: 'Habits' },
  { label: '更新日志', icon: 'ScaleToOriginal', route: 'Uplog' },
  { label: '关于系统', icon: 'InfoFilled', route: 'About' }
])

const goProfile = (type) => {
  router.push({
    path: '/profile',
    query: {
      type
    }
  })
}

const goView = (item) => {
  router.push({
    name: item.route
  })
}

const logout = () => {
  const email = localStorage.getItem('email')
  const password = localStorage.getItem('password')
  localStorage.clear()
  localStorage.setItem('email', email)
  localStorage.setItem('password', password)
  window.location.reload()
}
</script>

<style lang="scss">
.user-info {
  height: 100%;
  padding: 10px;
  overflow: hidden;
  display: flex;
  align-items: center;
  cursor: pointer;
  .user-info-avatar {
    height: 100%;
    border-radius: 50%;
    background: #c0c4cc;
  }
  .user-info-name {
    margin-left: 8px;
    color: #606266;
    font-size: 15px;
  }
  .user-info-role {
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 10px 5px;
    height: 18px;
    font-size: 12px;
    margin-left: 10px;
    word-break: keep-all;
    border: 1px solid rgb(16, 111, 255);
    color: rgb(16, 111, 255);
  }
  .user-info-caret {
    margin-left: 5px;
    padding-top: 5px;
    .el-icon {
      color: #707070;
    }
  }
}
.user-info-popover {
  transform: translate(-20px, 59px) !important;
  padding: 0 !important;
  background: #FFFFFF;
  box-shadow: 0px 0px 48px 2px rgba(0,0,0,0.2);
  padding: 0px !important;
  .el-popper__arrow {
    display: none;
  }
  .user-info__content {
    .popover__header {
      width: 100%;
      height: 80px;
      background-image: url('./images/user-info-popover-bg.png');
      border-bottom: 1px dashed #ddd;
      padding: 23px 20px 21px 23px;
      flex-shrink: 0;
      background-size: 100% 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      .header-left {
        display: flex;
        align-items: center;
        .user-avatar {
          width: 40px;
          height: 40px;
          border-radius: 50%;
        }
        > div {
          display: flex;
          flex-direction: column;
          margin-left: 10px;
          justify-content: center;
          .user-name {
            font-size: 14px;
            color: #333841;
          }
          .user-role {
            font-size: 12px;
            color: #808392;
          }
        }
      }
    }
    .popover__content {
      padding: 15px 20px;
      .personal-center {
        width: 100%;
        display: flex;
        align-items: center;
        padding: 0 40px;
        justify-content: space-between;
        .center-item {
          display: flex;
          flex-direction: column;
          justify-items: center;
          align-items: center;
          cursor: pointer;
          color: #666666;
          .el-icon {
            font-size: 30px;
          }
          > span {
            font-size: 14px;
            font-weight: 400;
          }
          &:hover {
            color: #409eff;
          }
        }
      }
      .entry-list {
        display: flex;
        flex-wrap: wrap;
        margin-top: 15px;
        .entry-item {
          width: 50%;
          height: 36px;
          background: #F8FAFC;
          border-radius: 2px;
          display: flex;
          justify-content: center;
          align-items: center;
          margin-bottom: 2px;
          color: #1c1f21;
          cursor: pointer;
          &:nth-child(odd) {
            border-right: 2px solid #fff;
          }
          .el-icon {
            font-size: 16px;
            margin-right: 8px;
          }
          &:hover {
            background: #efefef;
          }
        }
      }
    }
  }
}
</style>