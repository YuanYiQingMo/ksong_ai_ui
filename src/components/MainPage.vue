<template>
  <v-container>
    <v-app-bar :order="-1" title="化工智能学习平台"  style="background:linear-gradient(135deg, #333333, #ffffff);color:white; font-family: 'Brush Script MT', 'Cursive', sans-serif ;font-size: 35px;font-weight: bold; ">
      <template v-slot:prepend>
        <img height="100%" src="../assets/ai_logo.png" />
      </template>
    </v-app-bar>

  <v-navigation-drawer :v-model="true" style="background: linear-gradient(135deg, #333333, #ffffff)">
    <v-img src="../assets/ai_logo.png"  cover class="imgg"></v-img>
    <div class="drawer">
      <v-btn @click="dianji" class="click" icon="mdi-arrow-up-circle-outline"></v-btn>
         <sidermenu />
     </div>
    </v-navigation-drawer>

    <v-main
      class="d-flex align-center justify-center"
      style="min-height: 500px; "
    >
      <AIDialog :data="currentData"></AIDialog>
    </v-main>
  </v-container>
  <v-footer :app="true" style="background: linear-gradient(135deg, #333333, #ffffff)">
    <div style="width: 100%">
      <v-text-field label="问个问题吧">
        <template v-slot:append>
          <v-btn
            @click="upload_question"
            class="upload-btn"
            icon="mdi-arrow-up-circle-outline"
          >
          </v-btn>
        </template>
      </v-text-field>
    </div>
  </v-footer>
  <imageplot></imageplot>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import sidermenu from './sidermenu.vue';


let currentData = ref({ id: 0, text: `` });
let current_model_id = 1;

function upload_question() {}
function changeAiModel(id: number) {
  if (id == current_model_id) {
    return;
  }
  for (let item of aiData) {
    if (item.id == id) {
      currentData.value = item;
    //   console.log(currentData.value)
      return;
    }
  }
}
function dianji() {
  const drawer = document.getElementsByClassName('drawer')[0];
  console.log(drawer.classList)
  if(!drawer.classList.contains('addWidth')){
    console.log(drawer.classList.contains('addWidth'));
    console.log(drawer.classList);
    drawer.classList.add("addWidth")
    drawer.style.overflowY = 'auto';// 当侧边栏展开时，启用滚动条
  }
  else{
    drawer.classList.remove("addWidth")
    drawer.style.overflowY = 'hidden';// 当侧边栏关闭时，隐藏滚动条
  }

}
</script>

<style scoped>
.addWidth{
  height: 100vh!important;
  left: 0px!important;
  width: 256px!important;
  min-width: 256px!important;
  position: absolute;
  z-index: 11!important;
  top: 0px!important;
  bottom: 0px!important;
}
.drawer{
  height: 100vh!important;
  transition: width 1s;
  left: -256px;
  width: 256px;
  min-width: 256px;
  position: absolute;
  z-index: 11;
  height: calc(100vh - 2.5rem);
  transition: all 0.2s;
  background: linear-gradient(135deg, #333333, #ffffff);
  box-shadow: 0px 5px 5px rgba(11, 2, 5, 0.1);
  top: 0px;
  bottom: 0px;
}
.deep-blue {
  background-color: #003366; /* 深蓝色 */
  color: #fff; /* 字体颜色 */
}
.model-name {
  height: 70px;
  color: #ffffff;
  font-family: 'Arial', 'Helvetica', sans-serif;
  font-weight: bold;
  font-size: 16px;
  padding: 10px;
  transition: background-color 0.3s;
}
.model-name:hover {
  background-color: rgba(255, 255, 255, 0.1);  /* 悬停效果 */
}
.upload-btn {
  font-size: 1.5rem;
}
.click {
  position: absolute;
  width: 50px;
  height: 60px;
  right: -48px;
  background:linear-gradient(135deg, #333333, #ffffff);
  box-shadow: 0px 5px 5px rgba(11, 2, 5, 0.1);
  border-radius: 0px 6px 6px 0px;
  line-height: 40px;
  cursor: pointer;
  text-align: center;
  transform: rotate(90deg);
  transition: transform 0.3s;

}
.elevation-24 {
  color: white; 
  font-family: 'Arial', 'Helvetica', sans-serif;
  font-weight: bolder;
  }
.gradient-header {
  background: linear-gradient(135deg, #5e60eb 3%, #4282fa 100%); /* 深蓝色渐变 */
  color: #fff; /* 字体颜色设置为白色 */
  font-family: 'Brush Script MT', 'Cursive', sans-serif !important;/* 使用艺术字 */
  font-size: 24px; /* 字体大小 */
}
.ai-data-container {
  position: absolute; /* 将AI数据部分显示在左上角 */
  top: 20px; /* 距离顶部的距离 */
  left: 20px; /* 距离左侧的距离 */
  color: #ffffff; /* 字体颜色 */
}
.imgg {
  transform: rotate(90deg);
  margin-top: 300px;
  color: #b3b7be;

}
</style>