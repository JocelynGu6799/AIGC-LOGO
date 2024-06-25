<template>
    <div class="logomain" @contextmenu.prevent>
        <CommonHeader></CommonHeader>
        <p class="loadingtext" ref="loadingtext">
            {{ loadingprogress }}%
        </p>
        <div v-if="nowStep === 1">第一步</div>
        <div v-if="nowStep === 2">
            <div class="message-box">
                <p class="message-box-text">常用标签预设推荐，如果合适请勾选。</p>
            </div>
            <div class="item-container1">
                <div v-for="item in gridItems1" :key="item.name" class="xuanxiang1" @click="handleClick1(item.name, 1)"
                    :class="{ selected: selectedName1 === item.name }">
                    {{ item.name }}
                </div>
            </div>
            <div class="item-container2">
                <div v-for="item in gridItems4" :key="item.name" class="xuanxiang1" @click="handleClick1(item.name, 2)"
                    :class="{ selected: selectedName2 === item.name }">
                    {{ item.name }}
                </div>
            </div>
            <div class="item-container3">
                <img src="@/assets/images/校标矢量文件_1.png" alt="" ref="cuzlogo" class="cuzlogo" @click="toggleLogoSelection('cuzlogo')">
                <div v-for="item in gridItems5" :key="item.name" class="xuanxiang1" @click="handleClick1(item.name, 3)"
                    :class="{ selected: selectedName3 === item.name }">
                    {{ item.name }}
                </div>
                <img src="@/assets/images/工程中心LOGO_1.png" alt="" ref="logo2" class="logo2" @click="toggleLogoSelection('logo2')">
            </div>

            <div class="line"></div>

            <div class="item-container4">
                <!-- 颜色 -->
          
                <div v-for="item in gridItems2" :key="item.name" class="xuanxiang2" @click="handleClick2(item.name, 4)"
                    :class="{ selected: selectedName4 === item.name }" :style="`border-color: ${item.mycolor};color: ${selectedName4 === item.name?'white':item.mycolor};background-color: ${selectedName4 === item.name? item.mycolor : 'white'};`">
                    {{ item.name }}
                </div>
               
            </div>

            <div class="line"></div>

            <div class="item-container5">
                <div v-for="item in gridItems3" :key="item.name" class="xuanxiang3" @click="handleClick2(item.name, 5)"
                    :class="{ selected: selectedName5 === item.name }">
                    {{ item.name }}
                </div>
            </div>

        </div>

        <PageChangeComp :nowStep="nowStep" :total-steps="2" @change-step="handleStep" @start-create="handleCreate">
        </PageChangeComp>

    </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import axios from 'axios';
import { postGenerateApi,postLogo } from "@/api/generateApi";
import { getBlob } from "@/utils/getblob.js";
import { ElLoading } from "element-plus";
import { onMounted } from "vue";
import { getViewApi } from "@/api/userApi";
import { useDrawStore } from "@/stores/drawStore";
import { showNotify } from "vant";
import { fa } from "element-plus/es/locales.mjs";
import { addNumber } from "vant/lib/utils";
import base from "@/api/base";
const logoimg1=ref(null)
    const logoimg2=ref(null)
    const cuzlogo=ref(null)
    const logo2=ref(null)
onMounted(() => {
     //logo加载
   
     // 确保图片加载完成后再操作Canvas
     
        // const ctx = cuzlogo.value.getContext('2d');
        // cuzlogo.value.width = logoimg1.value.width;
        // cuzlogo.value.height = logoimg1.value.height;
        
        // // 绘制图片到 Canvas
        // ctx.drawImage(logoimg1.value, 0, 0);
        
        // // 获取绘制后的数据URL
        // const dataURL = cuzlogo.value.toDataURL();
        // console.log(dataURL);
    
});



//创建第一个网格数据
const gridItems1 = ref([
    { name: "三维" },
    { name: "简易" },
    { name: "抽象" },
    { name: "塔罗牌" },
    { name: "水彩" },
]);
const selectedName1 = ref( '');

const gridItems4 = ref([
    { name: "复古" },
    { name: "卡通" },
    { name: "油画" },
    { name: "新艺术" },
]);
const selectedName4 = ref( '');

const gridItems5 = ref([
    { name: "单色" },
    { name: "剪影" },
    { name: "强阴影" },
]);
const selectedName5 = ref( '');

//创建第二个网络数据
const gridItems2 = ref([
    { name: "红色", mycolor: "#FF0000" },
    { name: "橙色", mycolor: "#FFA500" },
    { name: "黄色", mycolor: "#F8DD50" },
    { name: "绿色", mycolor: "#008000" },
    { name: "蓝色", mycolor: "#0000FF" },
    { name: "紫色", mycolor: "#800080" },
    { name: "黑色", mycolor: "#000000" },
    { name: "棕色", mycolor: "#A52A2A" },
    { name: "粉色", mycolor: "#FFC0CB" },
    { name: "金色", mycolor: "#DEBD13" },
    { name: "银色", mycolor: "#C0C0C0" },
    { name: "白色", mycolor: "#D9D9D9" },
]);

const selectedName2 = ref( '');
//创建第三个网络数据
const gridItems3 = ref([
    { name: "猫" },
    { name: "云" },
    { name: "狗" },
    { name: "剑" },
    { name: "树" },
    { name: "鸟" },
    { name: "拳" },
    { name: "花" },
    { name: "太阳" },
    { name: "月亮" },
]);
const selectedName3 = ref( '');

const handleClick1 = (name, gridNumber) => {

    clearSelections();

    if (gridNumber === 1) {
        // 更新选中名称（第一个网格）
        selectedName1.value = name;
    } else if (gridNumber === 2) {
        selectedName2.value = name;
    } else if (gridNumber === 3) {
        selectedName3.value = name;
    } else if (gridNumber === 4) {
        selectedName4.value = name;
    } else if (gridNumber === 5) {
        selectedName5.value = name;
    }
};

const clearSelections = () => {
    selectedName1.value =  '';
    selectedName2.value =  '';
    selectedName3.value =  '';
}

const handleClick2 = (name, gridNumber) => {

    if (gridNumber === 4) {
        selectedName4.value = name;
    } else if (gridNumber === 5) {
        selectedName5.value = name;
    }
};

const drawStore = useDrawStore();
let voicetext=drawStore.voiceinput
voicetext=voicetext.substring(0, voicetext.length - 1);
const router = useRouter();

let nowStep = ref(2);

const handleStep = (mystep) => {

    // mystep的值为-1或1,对应改变nowStep的值
    console.log("mystep", mystep);
    nowStep.value += mystep;
    if (nowStep.value == 1 && mystep == -1) {
        //   window.location.reload();
        router.push('/logo')
    }
}

const mystring=ref('')
const websocket = ref(null);
    const receivedMessage = ref('');
   
    const loadingprogress=ref(0)
    const loadingtext=ref(null)
// 点击生成与后端交互
const handleCreate = () => {
   
    const loadingInstance = ElLoading.service({
    fullscreen: true,
    // text: loadingprogress.value+'%',
  });
  loadingtext.value.style.display='block'
  
    mystring.value=selectedName1.value + ',' + selectedName2.value + ',' + selectedName3.value + ',' + selectedName4.value + ',' + selectedName5.value;
    // router.push('/logo/view')
    console.log(selectedName1.value, selectedName2.value, selectedName3.value, selectedName4.value, selectedName5.value);
    const jsonData = {
        "client_id": "cuztest",
        "prompt": {
            "21": { "inputs": { "seed": Math.floor(1000000000000000 + Math.random() * 9000000000000000), "steps": 8, "cfg": 3, "sampler_name": "ddim", "scheduler": "sgm_uniform", "denoise": 1, "model": ["3", 0], "positive": ["1", 0], "negative": ["25", 0], "latent_image": ["23", 0] }, "class_type": "KSampler" },
            "22": { "inputs": { "ckpt_name": "sdXL_v10VAEFix.safetensors" }, "class_type": "CheckpointLoaderSimple" },
            "23": { "inputs": { "width": 1024, "height": 1024, "batch_size": 4 }, "class_type": "EmptyLatentImage" },
            "25": { "inputs": { "text": "", "clip": ["27", 1] }, "class_type": "CLIPTextEncode" },
            "26": { "inputs": { "samples": ["21", 0], "vae": ["22", 2] }, "class_type": "VAEDecode" },
            "27": { "inputs": { "lora_name": "LogoRedmondV2-Logo-LogoRedmAF.safetensors", "strength_model": 1, "strength_clip": 1, "model": ["60", 0], "clip": ["60", 1] }, "class_type": "LoraLoader" },
            "1": { "inputs": { "from_translate": "chinese simplified", "to_translate": "english", "add_proxies": "disable", "proxies": "", "auth_data": "", "service": "MyMemoryTranslator [free]", "text": "logo,"+voicetext+","+mystring.value, "Show proxy": "proxy_hide", "Show authorization": "authorization_hide", "clip": ["27", 1] }, "class_type": "DeepTranslatorCLIPTextEncodeNode" },
            "52": { "inputs": { "detail_method": "VITMatte(local)", "detail_erode": 1, "detail_dilate": 1, "black_point": 0.01, "white_point": 0.02, "process_detail": true, "image": ["26", 0] }, "class_type": "LayerMask: RmBgUltra V2" },
            "60": { "inputs": { "lora_name": "Hyper-SDXL-8steps-lora.safetensors", "strength_model": 0.7000000000000001, "strength_clip": 1, "model": ["22", 0], "clip": ["22", 1] }, "class_type": "LoraLoader" },
            "61": { "inputs": { "ipadapter_file": "ip-adapter_sdxl.safetensors" }, "class_type": "IPAdapterModelLoader" },
            "63": { "inputs": { "clip_name": "CLIP-ViT-bigG-14-laion2B-39B-b160k.safetensors" }, "class_type": "CLIPVisionLoader" },
            "3": { "inputs": { "weight": 0.6, "weight_type": "linear", "combine_embeds": "concat", "start_at": 0, "end_at": 1, "embeds_scaling": "V only", "model": ["27", 0], "ipadapter": ["61", 0], "image": ["2", 0], "clip_vision": ["63", 0] }, "class_type": "IPAdapterAdvanced" },
            "2": { "inputs": { "image": logoname.value?logoname.value:"KUMO.jpg", "upload": "image" }, "class_type": "LoadImage" },
            // x"2": { "inputs": { "image": "KUMO.jpg", "upload": "image" }, "class_type": "LoadImage" },
            "100": { "inputs": { "filename_prefix": "Logo", "images": ["52", 0] }, "class_type": "SaveImage" }
        }
    }
    postLogo(jsonData).then((res)=>{
        console.log("提交文字",res)
        websocket.value = new WebSocket('wss://u262838-87ee-75614327.westx.seetacloud.com:8443/ws?clientId=cuztest');
      websocket.value.onopen = () => {
        console.log('WebSocket connection established.');
      };
      websocket.value.onmessage = (event) => {
        console.log('Received message:', JSON.parse(event.data));
        receivedMessage.value =JSON.parse(event.data);
        // 可以根据需要进行其他处理
        if(receivedMessage.value.type==='executed'){
            drawStore.logooutputs=receivedMessage.value.data.output.images
            console.log("绘图成功",drawStore.logooutputs)
            // setTimeout(()=>{
                loadingInstance.close()
            router.push('/logo/view')
            // },500)
          
        }
        else{
            loadingprogress.value+=6
            console.log("loadingprogress.value",loadingprogress.value);
            loadingInstance.text=loadingprogress.value+'%'
        }
      };
      websocket.value.onclose = () => {
        console.log('WebSocket connection closed.');
        loadingInstance.close()

      };
      websocket.value.onerror = (error) => {
        console.error('WebSocket error:', error);
        loadingInstance.close()
        showNotify({ type: "danger", message: "绘图失败,请重试" });
        loadingtext.style.display='none'


      };

    })
    .catch((err)=>{
        console.log("提交文字失败",err)
        loadingInstance.close()
        showNotify({ type: "danger", message: "提交失败,请重试" });
        loadingtext.style.display='none'


    })

    


   
            
       

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
const logoname=ref('')
const toggleLogoSelection = (logoRef) => {
    clearLogoSelections();
    switch (logoRef) {
        case 'cuzlogo':
            cuzlogo.value.classList.add('logoselected');
            logoname.value='xiaohui.png'
            break;
        case 'logo2':
            logo2.value.classList.add('logoselected');
            logoname.value='zhongxin.jpg'

            break;
        default:
            break;
    }
};

const clearLogoSelections = () => {
    cuzlogo.value.classList.remove('logoselected');
    logo2.value.classList.remove('logoselected');
};


</script>

<style lang="scss" scoped>
.message-box {
    box-sizing: border-box;
    height: 15vw;
    width: 60vw;
    margin-left: 5vw;
    margin-top: 2vw;
    border: 1px solid #464646;
    box-shadow: 0px 2px 3.1px 2px rgba(0, 0, 0, 0.11);
    border-radius: 0px 15px 15px 15px;
}

.message-box-text {
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
    margin-top: -2vw;
}

.item-container1 {
    display: grid;
    grid-template-columns: repeat(5, 18vw);
    row-gap: 0vw;
    column-gap: -20vw;
    margin-top: 5vw;
    margin-left: 5vw;
}

.item-container1 .selected {
    background-color: white;
    background-image: url('@/assets/images/diamond1.png');
}

.item-container2 {
    display: grid;
    grid-template-columns: repeat(4, 18vw);
    row-gap: 0vw;
    column-gap: -20vw;
    margin-top: 0vw;
    margin-left: 15vw;
}

.item-container2 .selected {
    background-color: white;
    background-image: url('@/assets/images/diamond1.png');
}

.item-container3 {
    display: grid;
    grid-template-columns: repeat(3, 18vw);
    row-gap: 0vw;
    column-gap: -20vw;
    margin-top: 0vw;
    margin-left: 24vw;
}

.item-container3 .selected {
    background-color: white;
    background-image: url('@/assets/images/diamond1.png');
}

.item-container4 {
    display: grid;
    grid-template-columns: repeat(6, 15vw);
    grid-row-gap: 1.5vw;
    margin-top: 2vw;
    margin-left: 4vw;
    justify-items: center; /* 水平居中 */
    align-items: center; /* 垂直居中 */
}

.item-container4 .selected {
    background-color: #4768FF;
}

.item-container5 {
    display: grid;
    grid-template-columns: repeat(5, 15vw);
    row-gap: 2vw;
    column-gap: 2vw;
    margin-top: 2vw;
    margin-left: 6vw;
}

.item-container5 .selected {
    background-color: white;
    background-image: url('@/assets/images/label1.png');
}

.xuanxiang1 {
    position: relative;
    display: flex;
    /* 使用 Flexbox 布局 */
    justify-content: center;
    /* 水平居中 */
    align-items: center;
    /* 垂直居中 */
    width: 12vw;
    /* 确保整个区域可点击 */
    height: 12vw;
    border: none;
    /* 移除边框 */
    padding: 10px;
    font-size: 32px;
    cursor: pointer;
    overflow: hidden;
    /* 确保溢出部分被隐藏 */
    writing-mode: vertical-lr;
    /* 设置垂直书写模式 */
    text-orientation: mixed;
    /* 确保文字方向混合 */
    background-image: url('@/assets/images/diamond.png');
    /* 用你的图片路径替换 'src/assets/images/diamond.png' */
    background-size: cover;
    /* 确保背景图案覆盖整个区域 */
}

.xuanxiang2 {
    border: 2px solid #a1a1a1;
    border-radius: 5px;
    padding: 10px;
    text-align: center;
    width: 8vw;
    font-size: 32px;
    cursor: pointer;
}

.xuanxiang3 {
    position: relative;
    display: flex;
    /* 使用 Flexbox 布局 */
    justify-content: center;
    /* 水平居中 */
    align-items: center;
    /* 垂直居中 */
    width: 10vw;
    /* 确保整个区域可点击 */
    height: 4.5vw;
    border: none;
    /* 移除边框 */
    padding: 10px;
    font-size: 32px;
    cursor: pointer;
    overflow: hidden;
    /* 确保溢出部分被隐藏 */
    background-image: url('@/assets/images/label.png');
    background-size: cover;
    /* 确保背景图案覆盖整个区域 */
}

.selected {
    background-color: #92b0fd;
    // background:linear-gradient( #92b0fd, #f2f2f2);
    color: aliceblue
}

.line {
    border: 0.2vw solid #e0e0e0;
    margin-top: 2vw;
    margin-left: 5vw;
    width: 88vw;
}
.el-loading-spinner .el-loading-text {
    // color: var(--el-color-primary);
    font-size: 4vw !important;
    margin: 3px 0;
}
.item-container3 img {
    width: 14vw;
    // height: 12vw; /* 与宽度相同，保持正方形 */
    position: absolute;
    top:96.5vw;
    // background-color: #b52f2f;
    display: block;
    box-sizing: border-box;
}
.item-container3 .cuzlogo{
    left: 5vw;
    // top:103vw;


}
.logo2{
    left: 81vw;
}
.logobox,.cuzlogo img{
    position: absolute;
}
.logoselected{
    border: 5px solid #4768FF
}
.loadingtext{
    height: 6vw;
    width: 6vw;
    text-align: center;
    font-size: 4vw;
    position: absolute;
    left: 50%;
    top: 55%;
    margin-left: -3vw;
    margin-top: -3vw;
    color: #1989fa;
    z-index: 9999;
    display: none
}

</style>
