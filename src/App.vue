<template>
  <div
    class="bg-[url('assets/imgs/bg.jpg')] bg-cover bg-center h-screen text-white p-5 flex overflow-hidden"
    v-if="data"
  >
    <!-- 左 -->
    <div class="flex-1 mr-2 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
      <!-- 横向柱状图 -->
      <HorizontalBar
        class="h-1/3 box-border pb-4"
        :data="data.data.data.regionData"
      />
      <!-- 雷达图 -->
      <RadarBar class="h-1/3 box-border pb-4" :data="data.data.data.riskData" />
      <!-- 数据传递关系图 -->
      <Relation class="h-1/3" :data="data.data.data.relationData" />
    </div>
    <!-- 中 -->
    <div class="w-1/2 mr-2 flex flex-col">
      <!-- 数据展示图 -->
      <TotalData
        class="bg-opacity-50 bg-slate-800 p-3"
        :data="data.data.data.totalData"
      />
      <!-- 地图可视化 -->
      <MapChart
        class="bg-opacity-50 bg-slate-800 p-3 mt-4 flex-1"
        :data="data.data.data.mapData"
      />
    </div>
    <!-- 右 -->
    <div class="flex-1 bg-opacity-50 bg-slate-800 p-3 flex flex-col">
      <!-- 竖向柱状图 -->
      <VerticalBar
        class="h-1/3 box-border pb-4"
        :data="data.data.data.serverData"
      />
      <!-- 环形图 -->
      <RingBar
        class="h-1/3 box-border pb-4"
        :data="data.data.data.abnormalData"
      />
      <!-- 文档云图 -->
      <WordCloud
        class="h-1/3 box-border"
        :data="data.data.data.wordCloudData"
      />
    </div>
  </div>
</template>

<script setup>
import HorizontalBar from './components/HorizontalBar.vue';
import MapChart from './components/MapChart.vue';
import Relation from './components/Relation.vue';
import RadarBar from './components/RadarBar.vue';
import WordCloud from './components/WordCloud.vue';
import RingBar from './components/RingBar.vue';
import TotalData from './components/TotalData.vue';
import VerticalBar from './components/VerticalBar.vue';

import { ref } from 'vue';
import { getVisualization } from './api/visualization';

const data = ref(null);
const loadData = async () => {
  data.value = await getVisualization();
  console.log('all data:', data.value.data?.data);
};

loadData();
setInterval(() => {
  loadData();
}, 5000);
</script>

<style lang="scss" scoped></style>
