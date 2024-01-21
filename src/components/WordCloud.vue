<template>
  <div>[文档云图]</div>
  <div ref="target" class="w-full h-full"></div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue';
import * as echarts from 'echarts';
import 'echarts-wordcloud';
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});

console.log('WordCloud.props: ', props.data.abnormals);

let myChart = null;
const target = ref(null);
onMounted(() => {
  myChart = echarts.init(target.value);
  renderChart();
});

const randomRGB = () => {
  const r = Math.floor(Math.random() * 256);
  const g = Math.floor(Math.random() * 256);
  const b = Math.floor(Math.random() * 256);
  return `rgb(${r}, ${g}, ${b})`;
};

const renderChart = () => {
  const options = {
    series: [
      {
        type: 'wordCloud',
        sizeRange: [8, 46],
        rotationRange: [0, 0],
        gridSize: 0,
        layoutAnimation: true,
        textStyle: {
          color: randomRGB,
        },
        emphasis: {
          textStyle: {
            fontWeight: 'bold',
            color: '#000',
          },
        },
        data: props.data.datas,
      },
    ],
  };

  myChart.setOption(options);
};

watch(() => props.data, renderChart);
</script>
<style lang="scss"></style>
