<template>
  <div>
    <div>[云端报警风险]</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
import * as echarts from "echarts";
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});

console.log("Radars.props: ", props.data.risks);

let myChart = null;
const target = ref(null);
onMounted(() => {
  myChart = echarts.init(target.value);
  renderChart();
});

//2.构建option配置对象
const renderChart = () => {
  const options = {
    //雷达图的坐标系配置
    radar: {
      axisName: {
        color: "#05D5FF",
        fontSize: 14,
      },
      shape: "polygon",
      center: ["50%", "50%"],
      radius: "80%",
      startAngle: 120,
      axisLine: {
        lineStyle: {
          color: "rgba(5, 213, 255, 0.8)",
        },
      },
      splitLine: {
        show: true,
        lineStyle: {
          width: 1,
          color: "rgba(5, 213, 255, 0.8)",
        },
      },
      indicator: props.data.risks.map((risk) => ({
        name: risk.name,
        max: 100,
      })),
      splitArea: {
        show: false,
      },
    },
    //坐标极点
    polar: {
      center: ["50%", "50%"],
      radius: "0%",
    },
    //坐标角度
    angleAxis: {
      min: 0,
      interval: 5,
      clockwise: false,
    },
    //径向轴
    radiusAxis: {
      min: 0,
      interval: 20,
      splitLine: {
        show: true,
      },
    },
    //图标核心配置
    series: {
      //series只是一个对象表示只有一个图
      type: "radar",
      symbol: "circle",
      symbolSize: 10,
      itemStyle: {
        //normal: {
        color: "#05D5FF",
        //},
      },
      areaStyle: {
        //normal: {
        color: "#05D5FF",
        opacity: 0.5,
        //},
      },
      lineStyle: {
        width: 2,
        color: "#05D5FF",
      },
      label: {
        //normal: {
        show: true,
        color: "#fff",
        //},
      },
      data: [
        {
          value: props.data.risks.map((risk) => risk.value),
        },
      ],
    },
  };

  myChart.setOption(options);
};

watch(() => props.data, renderChart);
</script>
<style lang="scss"></style>
