<template>
  <div>
    <div>[服务资源占用比]</div>
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

console.log("VerticalBar.props: ", props.data);

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
      type: "category",
      data: props.data.servers.map((server) => server.name),
      axisLabel: {
        color: "#9eb1c8",
      },
    },
    yAxis: {
      type: "value",
      show: false,
      max: function (value) {
        return parseInt(value.max * 1.2);
      },
    },
    //图标绘制的位置， 上下左右
    grid: {
      top: 16,
      right: 0,
      bottom: 26,
      left: -26,
      containLabel: true,
    },
    series: [
      {
        type: "bar", //柱形图
        data: props.data.servers.map((server) => ({
          name: server.name,
          value: server.value,
        })),
        showBackground: false,

        itemStyle: {
          color: "#5d98ce",
          barBorderRadius: 5,
          shadowColor: "rgba(0, 0, 0, 0.3)",
          shadowBlur: 5,
        },
        barWidth: 12,
        label: {
          //显示的文字
          show: true,
          position: "top", //表示数据显示在顶上
          textStyle: {
            color: "#fff",
          },
          formatter: "{c}%", //c表示数据
        },
      },
    ],
  };

  myChart.setOption(options);
};

watch(() => props.data, renderChart);
</script>
<style lang="scss"></style>
