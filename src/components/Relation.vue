<template>
  <div>[数据传递关系图]</div>
  <div ref="target" class="w-full h-full"></div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue';
import * as echarts from 'echarts';
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});

console.log('Relation.props: ', props.data);

let myChart = null;
const target = ref(null);
onMounted(() => {
  myChart = echarts.init(target.value);
  renderChart();
});

const renderChart = () => {
  const options = {
    xAxis: {
      show: false,
      type: 'value',
    },
    yAxis: {
      show: false,
      type: 'value',
    },
    series: [
      {
        type: 'graph',
        layout: 'none',
        coordinateSystem: 'cartesian2d', //二维直角坐标系
        symbolSize: 26,
        z: 3,
        //边界标签文字
        edgeLabel: {
          normal: {
            show: true,
            color: '#fff',
            textStyle: {
              fontSize: 14,
            },
            //文本展示内容
            formatter: function (params) {
              return params.data.speed;
            },
          },
        },
        label: {
          normal: {
            show: true,
            position: 'bottom',
            color: '5E5E5E',
          },
        },
        edgeSymbol: ['none', 'arrow'], //数据下方线的箭头
        edgeSymbolSize: 8,
        data: props.data.relations.map((item) => {
          if (item.id !== 0) {
            return {
              name: item.name,
              category: 0,
              active: true,
              speed: `${item.speed}kb/s`,
              value: item.value,
            };
          } else {
            //数据中心的id为0
            return {
              name: item.name,
              value: item.value,
              symbolSize: 100,
              itemStyle: {
                normal: {
                  color: {
                    colorStop: [
                      {
                        offset: 0,
                        color: '#157eff',
                      },
                      {
                        offset: 1,
                        color: '#35c2ff',
                      },
                    ],
                  },
                },
              },
              label: {
                normal: {
                  fontSize: 14,
                },
              },
            };
          }
        }),
        links: props.data.relations.map((item, index) => ({
          source: item.source,
          target: item.target,
          speed: `${item.speed}kb/s`,
          lineStyle: {
            normal: {
              color: '#12b5d0',
              curveness: 0.2,
            },
          },
          label: {
            show: true,
            offset: [10, 0],
          },
        })),
      },
      {
        type: 'lines',
        coordinateSystem: 'cartesian2d',
        z: 1,
        effect: {
          //线的特效
          show: true,
          smooth: false,
          trailLength: 0,
          symbol: 'arrow',
          color: 'rgba(55, 155, 255, 0.6)',
          symbolSize: 12,
        },
        lineStyle: {
          normal: {
            curveness: 0.2,
          },
        },
        // 线的数据级，前后线需要重合。数据固定
        data: [
          [{ coord: [0, 300] }, { coord: [50, 200] }],
          [{ coord: [0, 100] }, { coord: [50, 200] }],
          [{ coord: [50, 200] }, { coord: [100, 100] }],
          [{ coord: [50, 200] }, { coord: [100, 300] }],
        ],
      },
    ],
  };

  myChart.setOption(options);
};

watch(() => props.data, renderChart);
</script>
<style lang="scss"></style>
