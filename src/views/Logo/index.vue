<template>
  <div class="logomain">
    <CommonHeader></CommonHeader>
    <div>
      <div class="message-box">
        <p class="message-box-text">请向我描述您所需的logo模样。</p>
      </div>
      <p class="example-title">案例参考</p>
      <p class="example-text">小鸭子坐在火车上&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;三层简约蛋糕切片
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;三文鱼和叉子
        <br>小猫坐在船上，旁边散落一些花瓣</p>
      <div class="transcription-box" v-if="transcription !== ''">
        <div id="transcription" class="record">
          <span class="record-text">{{ transcription }}</span>
        </div>
      </div>
      <div class="ripple-container" v-if="showRipple">
        <div class="ripple"></div>
      </div>
      <img ref="recordButton" @touchstart="startRecording" @touchend="stopRecording" src="@/assets/images/voice.png" class="record-button"/>
    </div>
    <div class="buttons" v-if="button">
      <VanButton class="resetbtn" @click="handleReset">重新输入</VanButton>
        <VanButton class="createbtn"  @click="router.push('/logo/select')">下一步</VanButton>
    </div>
    
    <!-- <div v-if="nowStep===2">第二步</div>
    <div v-if="nowStep===3">第三步</div>
    <div v-if="nowStep===4">第四步</div>
    <PageChangeComp :nowStep="nowStep" :total-steps="4" @change-step="handleStep" @start-create="handleCreate" v-if="button"></PageChangeComp> -->

  </div>
</template>

<script setup>
import { ref} from "vue";
import { useRouter } from "vue-router";
import axios from 'axios';
import fs from 'fs';
import { onBeforeUnmount } from 'vue';
import { onMounted } from "vue";
import { postGenerateApi } from "@/api/generateApi";
import { getBlob } from "@/utils/getblob.js";
import { ElLoading } from "element-plus";
import { getViewApi } from "@/api/userApi";
import { showNotify } from "vant";
import { fa } from "element-plus/es/locales.mjs";
import { addNumber } from "vant/lib/utils";
import { useDrawStore } from "@/stores/drawStore";
const router = useRouter();
let nowStep = ref(1);
let showRipple = ref(false);
const handleReset = () => {
    // 刷新页面
    window.location.reload();
    // router.push('/logo')
}
const handleStep = (mystep) => {
  
  // mystep的值为-1或1,对应改变nowStep的值
  console.log("mystep", mystep);
  nowStep.value += mystep;
  if(nowStep.value==1 && mystep==-1){
    window.location.reload();
  }
}

const recordButton = ref(false);
const transcription = ref('');
const showTranscription = ref(false);
let recognition = new webkitSpeechRecognition();
recognition.lang = 'zh-CN'; // 设置语言为中文

let isRecording = false;
let button = false;
let lastTranscription = '';
const drawStore=useDrawStore()
const startRecording = () => {
  isRecording = true;
  showTranscription.value = true;
  transcription.value = '......';

  showRipple.value = true;

  recognition.start();

  recognition.onresult = function(event) {
    let result = event.results[0][0].transcript;
    transcription.value = result;
    drawStore.voiceinput=result
    lastTranscription = result;
    button=true;
    recordButton.value.style.display = "none";
    
  }

  recognition.onerror = function(event) {
    transcription.value = '发生错误，请重试。';
    
    showRipple.value = false;
    
  }
}

const stopRecording = () => {
  isRecording = false;
  recognition.stop();

  showRipple.value = false;

  

  if (lastTranscription.trim() === '') {
    showTranscription.value = false;
  }
}

// 点击生成与后端交互
const handleCreate = () => {
router.push('/logo/view')
  // let myweight = 0.6
  // //  不存在图片weight就为0
  // // 如果有自定义颜色,添加到颜色数组
  // if (mycolor.value) {
  //   selectedName3.value.push(mycolor.value);
  // }

  // var fd = new FormData();

  // fd.append("logoKeys", logokeysarr.value);
  // fd.append("language", "chinese simplified");
  // fd.append("weight", myweight);
  // fd.append("prompt", "Custom_Logo");
  // fd.append("client", "cuz");
  // // getViewApi({prompt_id: "64e8f292-3db5-41cc-b9fa-b37a4e128450", client_id: "client_id_argv" })
  // // .then((res)=>console.log(res))
  // console.log("执行生成逻辑", bodyImg, logokeysarr.value);
  // postGenerateApi(fd, { product: "logo" })
  //   .then((postres) => {
  //     console.log("posterupload res", postres);

  //     const intervalId = setInterval(() => {
  //       if (calledGetViewApi.value) {
  //         console.log("calledGetViewApi", calledGetViewApi.value);

  //         getViewApi({ prompt_id: postres.prompt_id, client_id: "cuz" })
  //           .then((response) => {
  //             console.log("view res", response);
  //             if (response.statusCode === 200) {
  //               console.log("绘图成功", response);

  //               const keys = Object.keys(response.data); // 获取对象的所有键
  //               const firstKey = keys[0]; // 获取数组中的第一个键
  //               const secondKey = keys[1]; // 获取数组中的第一个键
  //               const imgurl1 = response.data[firstKey]; // 获取第一个键对应的值
  //               const imgurl2 = response.data[secondKey]; // 获取第一个键对应的值
  //               console.log("imgurl,", imgurl1, imgurl2);
  //               drawStore.logoimgurl1 = imgurl1;
  //               drawStore.logoimgurl2 = imgurl2;

  //               loadingInstance.close();
  //               calledGetViewApi.value = false;

  //               clearInterval(intervalId);
  //               router.push("/logo/view");
  //             } else if (response.statusCode === 400) {
  //               console.log("等待绘图中...");
  //             } else {
  //               console.log("绘图失败");
  //               loadingInstance.close();
  //               // calledGetViewApi.value = false

  //               clearInterval(intervalId);
  //               showNotify({ type: "danger", message: "绘图失败,请重试" });

  //             }
  //           })
  //           .catch((error) => {
  //             console.error("获取绘图数据失败:", error);
  //             loadingInstance.close();
  //             clearInterval(intervalId);
  //             // showNotify({ type: "danger", message: "网络错误" });

  //             // calledGetViewApi.value = false

  //             // setTimeout(()=>{
  //             //     router.push("/")
  //             // },1000)
  //           });
  //       }
  //     }, 2000);
  //   })
  //   .catch((error) => {
  //     console.error("获取上传数据失败:", error);
  //     showNotify({ type: "danger", message: "网络错误" });
  //     loadingInstance.close();
  //   });
};

</script>

<style lang="scss" scoped>

.message-box{
box-sizing: border-box;
height: 20vw;
width: 60vw;
margin-left: 5vw;
margin-top: 2vw;
border: 1px solid #464646;
box-shadow: 0px 2px 3.1px 2px rgba(0, 0, 0, 0.11);
border-radius: 0px 15px 15px 15px;
}

.message-box-text{
width: 55vw;
height: 30vw;
padding: 3.5vw;
font-family: 'Source Han Sans CN VF';
font-style: normal;
font-weight: bold;
font-size: 4vw;
line-height: 6vw;
letter-spacing: 0.1vw;
color: #585858;
}

.example-title{
margin-left: 6.5vw;
margin-top: 1.5vw;
font-family: 'Inter';
font-style: normal;
font-size: 2.5vw;
line-height: 6vw;
letter-spacing: 0.1vw;
color: #4D4D4D;
}

.example-text{
margin-left:7.5vw;
margin-top: -0.5vw;
font-family: 'Source Han Sans CN VF';
font-style: normal;
font-size: 2.5vw;
line-height: 6vw;
letter-spacing: 0.1vw;
color: #252525;
}


.record-button {
position: fixed;
left: 46vw;
top: 150vw;
}

.ripple-container {
  top: 50vw;
  left: 50vw;
  width: 100%;
  height: 100%;
  background-color: #ffffff;
}

.ripple {
  margin-top: 33vw;
  margin-left: 51vw;
  width: 20px;
  height: 20px;
  background-color: #4B6BFF;
  border-radius: 50%;
  transform: scale(6);
  animation: rippleEffect 1.5s linear infinite;
}

@keyframes rippleEffect {
  to {
    transform: scale(10);
    opacity: 0;
  }
}

.record{
box-sizing: border-box;
position: absolute;
max-width: 50vw;
padding: 10px;
border: 0.2vw solid #4B6BFF;
box-shadow: 0px 2px 3.1px 2px rgba(0, 0, 0, 0.11);
border-radius: 15px 0px 15px 15px;
overflow-wrap: break-word;
text-orientation: sideways-right;
transform-origin: top right; /* 指定变换的原点 */
}

.transcription-box {
margin-top: 5vw;
margin-left: -8vw;
width: 100%;
height: 30vw;
display: flex;
justify-content: flex-end; /* 框位于画面最右侧 */
align-items: flex-start;
overflow: hidden;
}

.record-text{
font-size: 5vw;
padding: 3vw;
font-family: 'Source Han Sans CN VF';
font-style: normal;
font-weight: 300;
font-size: 4vw;
line-height: 6vw;
letter-spacing: 0.1vw;
display: flex;
align-items: center;
color: #585858;
}
.van-button {
    width: 32.4vw;
    height: 9.72vw;
    font-size: 4vw;
    font-weight: bold;
    /* letter-spacing: 5px; */
    position: absolute;
    bottom: 13.888vw;
    border-radius: 1vw;
    /* top: 730px; */
    border:0.5vw solid #4768FF;



}

.laststep,
.resetbtn {
    /* background-color: #E3E3E3; */
    color: #4768FF;
    left: 10.42vw;



}

.nextstep,
.createbtn {
    background-color: #4768FF;
    color: white;
    right: 10.42vw;
    /* bottom: 85px; */



}
</style>
