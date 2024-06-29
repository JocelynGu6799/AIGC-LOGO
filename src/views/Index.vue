<template>
    <div>
      <div class="container">
        <transition name="fade">
          <div v-if="showDetails" class="details">
            <p>通过普通输入以及其它的途径，来实现生成是LOGO的效果</p>
          </div>
        </transition>
  
        <transition name="fade">
          <div v-if="!showDetails" class="summary">
            <VanButton @click="toggleDetails" class="project-button">项目详情</VanButton>
          </div>
        </transition>
  
        <transition name="button-move">
          <VanButton @click="gotostart" :class="{'move-down': showDetails}" class="start-button">开始生成我的LOGO</VanButton>
        </transition>
  
        <div class="message">
          <p style="margin-bottom: 0.4vh;">指导老师：张随雨</p>
          <p>团队成员</p>
          <p>桑菲阳 徐嘉杞 贾贝希 王紫茹 汪瑾</p>
          <p>李牧展 顾菁仪 宋鹏伟 张洛 杨云戈</p>
        </div>
  
        <img src="@/assets/images/background.png" style="height: 100vh;width: 100vw;" alt="">
        <img src="@/assets/images/logo.png" style="position: absolute;left: 0;width: 80vw;margin: 0 2vw;" alt="">
        <img src="@/assets/images/title.png" style="position: absolute;top: 15vh; left: 14vw; width: 70vw; margin: 0 2vw;" alt="">
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  import useUserStore from "@/stores/userStore";
  
  let userStore = useUserStore();
  const router = useRouter();
  const showDetails = ref(false);
  
  const gotostart = () => {
    router.push("/logo");
  }
  
  const toggleDetails = () => {
    showDetails.value = !showDetails.value;
  }

  //调用自己设计的动效按钮组件
  import { createApp } from 'vue'
  const app = createApp({});
  import NorButton from '@/components/NorButton.vue';
  app.component('NorButton', NorButton);

  </script>
  
  <style scoped>
  .container {
    position: relative;
    display: flex;
    justify-content: center;
    width: 100vw;
    height: 100vh;
  }
  
  .van-button {
    width: 65vw; 
    height: 5.5vh;
    font-size: 4vw;
    color: white;
    font-family: '方正黑体简体';
    font-weight: bold;
    border: none;
    border-radius: 10px;
    letter-spacing: 1vw;
    position: absolute;
    background-color: rgba(255, 255, 255, 0.312);
  }
  
  .project-button {
    top: 55vh;
    width: 55vw;
  }
  
  .start-button {
    top: 65vh;
    transition: top 0.5s ease;
  }
  
  .move-down {
    top: 70vh; /* 向下移动 5vh */
  }
  
  .container .message {
    position: absolute;
    display: flex;
    flex-direction: column;
    align-items: center;
    bottom: 3vh;
  }
  
  .container p {
    position: relative;
    font-family: '方正黑体简体';
    color: white;
    font-size: 2.5vw; 
    letter-spacing: 0.5vw;
  }
  
  /* 定义淡入淡出动画 */
  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s;
  }
  
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  
  .button-move-enter-active, .button-move-leave-active {
    transition: top 0.5s ease;
  }
  
  .details {
    position: absolute;
    font-family: '方正黑体简体';
    top: 42vh;
    padding: 10px;
    width: 75vw;
    letter-spacing: 0.8vw;
  }
  
  .summary {
    position: absolute;
    width: 100%;
    height: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  </style>
  