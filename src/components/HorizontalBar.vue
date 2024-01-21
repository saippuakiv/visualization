<template>
  <div>
    <div>[大区数据信息]</div>
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

console.log("horizontalBar, props", props.data);
//1.初始化echarts实例
let myChart = null;
const target = ref(null);
onMounted(() => {
  myChart = echarts.init(target.value);
  renderChart();
});

//2.构建option配置对象
const renderChart = () => {
  const options = {
    xAxis: {
      show: false,
      type: "value",
      max: function (value) {
        return parseInt(value.max * 1.2);
      },
    },
    yAxis: {
      type: "category",
      data: props.data.regions.map((region) => region.name),
      inverse: true,
      axisLine: {
        show: false,
      },
      axisTick: {
        show: false,
      },
      axisLabel: {
        color: "#9eb1c8",
      },
    },
    //图标绘制的位置， 上下左右
    grid: {
      top: 0,
      right: 0,
      bottom: 0,
      left: 0,
      containLabel: true,
    },
    series: [
      {
        type: "bar", //柱形图
        data: props.data.regions.map((region) => ({
          name: region.name,
          value: region.value,
        })),
        showBackground: true,
        backgroundStyle: {
          color: "rgba(180, 180, 180, 0.2)",
        },
        itemStyle: {
          color: "#5d98ce",
          barBorderRadius: 5,
          shadowColor: "rgba(0, 0, 0, 0.3)",
          shadowBlur: 5,
        },
        barWidth: 12,
        label: {
          show: true,
          position: "right",
          textStyle: {
            color: "#fff",
          },
        },
      },
    ],
  };

  myChart.setOption(options);
};

watch(
  () => props.data,
  () => {
    renderChart();
  }
);
</script>
<style lang="scss"></style>
