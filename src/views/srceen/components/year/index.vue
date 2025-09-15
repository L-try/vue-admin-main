<template>
  <div class="box7">
    <div class="title">
      <p>年度游客量对比</p>
      <img src="../../images/dataScreen-title.png" alt="" />
    </div>
    <div class="charts" ref="charts"></div>
  </div>
</template>

<script setup lang="ts">
defineOptions({
  name: 'YearPage',
})
import * as echarts from 'echarts'
import { ref, onMounted, watchEffect } from 'vue'

// 获取图表DOM节点
const charts = ref<HTMLElement | null>(null)
// 图表实例
let myChart: echarts.ECharts | null = null

// 组件挂载完毕后初始化图表
onMounted(() => {
  if (charts.value) {
    // 初始化图表实例
    myChart = echarts.init(charts.value)
    // 设置图表配置
    setChartOptions()

    // 监听窗口大小变化，自动调整图表尺寸
    window.addEventListener('resize', handleResize)

    // 组件卸载时清理
    onUnmounted(() => {
      window.removeEventListener('resize', handleResize)
      myChart?.dispose()
    })
  }
})

// 处理窗口大小变化
const handleResize = () => {
  myChart?.resize()
}

// 设置图表配置项
const setChartOptions = () => {
  if (!myChart) return

  // 模拟年度数据（更贴合实际业务）
  const yearData = [
    { year: '2010', value: 320 },
    { year: '2011', value: 450 },
    { year: '2012', value: 580 },
    { year: '2013', value: 620 },
    { year: '2014', value: 880 },
    { year: '2015', value: 950 },
    { year: '2016', value: 1200 },
    { year: '2017', value: 1500 },
    { year: '2018', value: 1800 },
    { year: '2019', value: 2100 },
    { year: '2020', value: 900 }, // 2020年特殊情况
    { year: '2021', value: 1600 },
    { year: '2022', value: 1900 },
    { year: '2023', value: 2500 },
  ]

  // 图表配置项
  const options = {
    // 图表背景透明，适配外层背景图
    backgroundColor: 'transparent',

    // 标题样式优化
    title: {
      text: '年度游客量趋势',
      left: 'center',
      textStyle: {
        color: '#fff',
        fontSize: 16,
        fontWeight: 'normal',
        textShadow: '0 0 3px rgba(0, 255, 255, 0.5)', // 文字发光效果
      },
    },

    // 提示框样式
    tooltip: {
      trigger: 'item',
      backgroundColor: 'rgba(10, 20, 40, 0.8)', // 半透明深色背景
      borderColor: 'rgba(0, 255, 255, 0.5)',
      borderWidth: 1,
      textStyle: { color: '#fff' },
      formatter: ({ name, value }: any) => `${name}年\n游客量: ${value} 人次`,
    },

    // 网格调整，让图表更居中
    grid: {
      left: '5%',
      right: '5%',
      top: '20%',
      bottom: '15%',
      containLabel: true, // 确保标签不被裁剪
    },

    // X轴样式
    xAxis: {
      type: 'category',
      data: yearData.map((item) => item.year),
      axisLine: {
        lineStyle: { color: 'rgba(255, 255, 255, 0.3)' }, // 轴线颜色
      },
      axisLabel: {
        color: 'rgba(255, 255, 255, 0.7)', // 标签颜色
        rotate: 30, // 文字旋转，防止重叠
        fontSize: 12,
      },
      splitLine: {
        show: true,
        lineStyle: { color: 'rgba(255, 255, 255, 0.1)' }, // 分割线颜色
      },
    },

    // Y轴样式
    yAxis: {
      type: 'value',
      show: true, // 显示Y轴便于查看数值
      axisLine: {
        lineStyle: { color: 'rgba(255, 255, 255, 0.3)' },
      },
      axisLabel: {
        color: 'rgba(255, 255, 255, 0.7)',
        fontSize: 12,
        formatter: (value: number) => {
          // 数值格式化，大于1000显示为k
          return value >= 1000 ? value / 1000 + 'k' : value
        },
      },
      splitLine: {
        lineStyle: { color: 'rgba(255, 255, 255, 0.1)' },
      },
      // 坐标轴最小值从0开始
      min: 0,
    },

    // 系列数据配置
    series: [
      {
        type: 'scatter',
        data: yearData.map((item) => ({
          name: item.year,
          value: item.value,
        })),
        // 散点大小根据数值动态变化，更直观
        symbolSize: (value: any) => 8 + value[0] / 300,
        // 散点样式
        itemStyle: {
          color: {
            type: 'radial', // 径向渐变
            x: 0.5,
            y: 0.5,
            r: 0.8,
            colorStops: [
              { offset: 0, color: 'rgba(0, 255, 255, 0.9)' },
              { offset: 1, color: 'rgba(0, 100, 255, 0.6)' },
            ],
          },
          borderColor: 'rgba(255, 255, 255, 0.8)',
          borderWidth: 1,
          shadowBlur: 10, // 阴影效果
          shadowColor: 'rgba(0, 255, 255, 0.5)',
        },
        // 标签样式优化
        label: {
          show: true,
          position: 'top',
          color: 'rgba(255, 255, 255, 0.9)',
          fontSize: 12,
          formatter: (params: any) => {
            // 只显示较大数值的标签，避免拥挤
            return params.value >= 1000 ? params.value : ''
          },
        },
        // 鼠标悬停效果
        emphasis: {
          itemStyle: {
            shadowBlur: 20,
            shadowColor: 'rgba(0, 255, 255, 0.8)',
            scale: 1.2, // 悬停时放大
          },
        },
      },
      // 添加趋势线，更直观展示变化趋势
      {
        type: 'line',
        data: yearData.map((item) => item.value),
        symbol: 'none', // 不显示线上的点
        lineStyle: {
          color: 'rgba(255, 100, 150, 0.6)',
          width: 2,
          type: 'dashed', // 虚线样式
        },
        smooth: true, // 平滑曲线
      },
    ],
  }

  // 设置图表配置
  myChart.setOption(options)
}

// 组件卸载时清理
const onUnmounted = (fn: () => void) => {
  // 模拟Vue的onUnmounted生命周期
  window.addEventListener('beforeunload', fn)
}
</script>

<style scoped lang="scss">
.box7 {
  width: 100%;
  height: 100%;
  background: url(../../images/dataScreen-main-rc.png) no-repeat;
  background-size: 100% 100%; /* 保持原有背景大小样式 */
  margin: 0 0 30px 0;

  .title {
    p {
      color: white;
      font-size: 18px;
    }
  }

  .charts {
    height: calc(100% - 30px);
    width: 100%; /* 确保图表宽度充满容器 */
  }
}
</style>
