<template>
     <div class="user-box">
       <!-- 用户基本信息面板 -->
       <div class="user-card">
         <!-- 用户头像、姓名 -->
         <van-cell>
           <!-- 使用 title 插槽来自定义标题 -->
           <template #icon>
             <img :src="Mydetails.photo" alt="" class="avatar" />
           </template>
           <template #title>
             <span class="username">{{Mydetails.name}}</span>
           </template>
           <template #label>
             <van-tag color="#fff" text-color="#007bff">申请认证</van-tag>
           </template>
         </van-cell>
         <!-- 动态、关注、粉丝 -->
         <div class="user-data">
           <div class="user-data-item">
             <span>{{Mydetails.art_count}}</span>
             <span>动态</span>
           </div>
           <div class="user-data-item">
             <span>{{Mydetails.follow_count}}</span>
             <span>关注</span>
           </div>
           <div class="user-data-item">
             <span>{{Mydetails.like_count}}</span>
             <span>粉丝</span>
           </div>
         </div>
       </div>

       <!-- 操作面板 -->
       <van-cell-group class="action-card">
         <van-cell icon="edit" title="编辑资料" is-link to="/user_edit" />
         <van-cell icon="chat-o" title="费启鸣" is-link @click="$router.push('/chat')" />
         <van-cell icon="warning-o" title="退出登录" is-link @click="signOut"/>
       </van-cell-group>
     </div>
   </template>

<script>
// 导入vant的提示框
import { Dialog } from 'vant'
// 导入个人信息详情
import { GetUserinfoAPI } from '@/api/index.js'
// 导入tokenjs
import { removeToken } from '@/utils/token'
export default {
  name: 'User',
  data() {
    return {
      Mydetails: {}
    }
  },
  methods: {
    signOut() {
      Dialog.confirm({
        title: '登录',
        message: '是否退出登录'
      })
        .then(() => {
          // on confirm
          removeToken()
          localStorage.removeItem('refresh_token')
          this.$router.replace('/login')
        })
        .catch(() => {
          // on cancel
        })
    }
  },
  async activated() {
    const { data: res } = await GetUserinfoAPI()
    this.Mydetails = res.data
    this.$store.commit('SET_USERPHOTO', this.Mydetails.photo)
  }
}
</script>

<style lang="less" scoped>
   .user-box {
     .user-card {
       background-color: #007bff;
       color: white;
       padding-top: 20px;
       .van-cell {
         background: #007bff;
         color: white;
         &::after {
           display: none;
         }
         .avatar {
           width: 60px;
           height: 60px;
           background-color: #fff;
           border-radius: 50%;
           margin-right: 10px;
         }
         .username {
           display: block;
           margin: 10px 0 0 0;
           font-size: 14px;
           font-weight: bold;
         }
       }
     }
     .user-data {
       display: flex;
       justify-content: space-evenly;
       align-items: center;
       font-size: 14px;
       padding: 30px 0;
       .user-data-item {
         display: flex;
         flex-direction: column;
         justify-content: center;
         align-items: center;
         width: 33.33%;
       }
     }
   }
   </style>
