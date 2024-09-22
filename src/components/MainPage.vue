<template>
  <v-container>
    <v-app-bar :order="-1" title="化工智能学习平台">
      <template v-slot:prepend>
        <img height="100%" src="../assets/ai_logo.png" />
      </template>
    </v-app-bar>
    <v-navigation-drawer :v-model="true">
      <v-list v-model:selected="current_model_id">
        <v-list-item class="elevation-24" title="便准规范"></v-list-item>
        <v-divider></v-divider>
        <v-list-item
          v-for="item in StandardItem"
          :key="item.value"
          link
          class="model-name"
          :value="item.value"
          @click="changeAiModel(item.value)"
        >
          {{ item.name }}
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-main
      class="d-flex align-center justify-center"
      style="min-height: 300px"
    >
      <AIDialog :data="currentData"></AIDialog>
    </v-main>
  </v-container>
  <v-footer :app="true">
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
</template>

<script setup lang="ts">
import { ref } from 'vue';

const StandardItem = [
  {
    name: "通用要求",
    value: 1,
  },
  {
    name: "材料",
    value: 2,
  },
  {
    name: "设计",
    value: 3,
  },
  {
    name: "制造、检验和验收",
    value: 4,
  },
  {
    name: "压力容器设计模型",
    value: 5,
  },
  {
    name: "塔器设计模型",
    value: 6,
  },
  {
    name: "换热器设计模型",
    value: 7,
  },
];

const aiData = [
  {
    id: 1,
    text: `为了保障固定式压力容器安全使用，预防和减少事故，保护人民生命和财产安
全，促进经济社会发展，根据《中华人民共和国特种设备安全法》《特种设备安全监
察条例》，制定本规程。`,
  },
  {
    id: 2,
    text: `(1)压力容器的选材应当考虑材料的力学性能、物理性能、工艺性能和与介质的
相容性；
(2)压力容器材料的性能、质量、规格与标志，应当符合相应材料的国家标准或
者行业标准的规定；
(3)压力容器材料制造单位应当在材料的明显部位作出清晰、牢固的出厂钢印标
志或者采用其他可以追溯的标志；
(4)压力容器材料制造单位应当向材料使用单位提供质量证明书，材料质量证明
书的内容应当齐全、清晰并且印制可以追溯的信息化标识，加盖材料制造单位质量
检验章；可以追溯的信息包括材料制造单位名称、材料牌号、规格、炉批号、交货状
态、质量证明书签发日期等内容；可以追溯的信息化标识包括二维码、条码等。
(5)压力容器制造、改造、修理单位从非材料制造单位取得压力容器材料时，应
当取得材料制造单位提供的质量证明书原件或者加盖了材料经营单位公章和经办负
责人签字(章)的复印件；
(6)压力容器制造、改造、修理单位应当对所取得的压力容器材料及材料质量证
明书的真实性和一致性负责；
(7)非金属压力容器制造单位应当有可靠的方法确定原材料或者压力容器成型
后的材质在腐蚀环境下使用的可靠性，必要时进行试验验证。`,
  },
  {
    id: 3,
    text: `(1)设计单位及其主要负责人对压力容器的设计质量负责；
(2)压力容器设计单位的资质、设计类别、品种和范围应当符合有关安全技术规
范的规定；
(3)压力容器的设计应当符合本规程的基本安全要求，对于采用国际标准或者境
外标准设计的压力容器，进行设计的单位应当向市场监管总局提供设计文件符合本
规程基本安全要求的符合性申明及比照表；
(4)设计单位应当向设计委托方提供本规程 3.1.4.1 规定的设计文件。`,
  },
  {
    id: 4,
    text: `(1)压力容器制造(含现场制造、现场组焊、现场粘接，注 4-1)单位应当取得特
种设备制造许可证，按照批准的范围进行制造，依据有关法规、安全技术规范的要
求建立压力容器质量保证体系并且有效运行，制造单位及其主要负责人对压力容器
的制造质量负责；
(2)制造单位应当严格执行有关法规、安全技术规范及技术标准，按照设计文件
的技术要求制造压力容器。`,
  },
  {
    id: 5,
    text: `(1)压力容器制造(含现场制造、现场组焊、现场粘接，注 4-1)单位应当取得特
种设备制造许可证，按照批准的范围进行制造，依据有关法规、安全技术规范的要
求建立压力容器质量保证体系并且有效运行，制造单位及其主要负责人对压力容器
的制造质量负责；
(2)制造单位应当严格执行有关法规、安全技术规范及技术标准，按照设计文件
的技术要求制造压力容器。`,
  },
  {
    id: 6,
    text: `(1)压力容器制造(含现场制造、现场组焊、现场粘接，注 4-1)单位应当取得特
种设备制造许可证，按照批准的范围进行制造，依据有关法规、安全技术规范的要
求建立压力容器质量保证体系并且有效运行，制造单位及其主要负责人对压力容器
的制造质量负责；
(2)制造单位应当严格执行有关法规、安全技术规范及技术标准，按照设计文件
的技术要求制造压力容器。`,
  },
  {
    id: 7,
    text: `(1)压力容器制造(含现场制造、现场组焊、现场粘接，注 4-1)单位应当取得特
种设备制造许可证，按照批准的范围进行制造，依据有关法规、安全技术规范的要
求建立压力容器质量保证体系并且有效运行，制造单位及其主要负责人对压力容器
的制造质量负责；
(2)制造单位应当严格执行有关法规、安全技术规范及技术标准，按照设计文件
的技术要求制造压力容器。`,
  },
];

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
</script>

<style scoped>
.model-name {
  height: 70px;
}
.upload-btn {
  font-size: 1.5rem;
}
</style>
