<template>
  <div class="box5">
    <div class="title">
      <p>未来七天游客数量趋势图</p>
      <img src="../../images/dataScreen-title.png" alt="" />
    </div>
    <div class="charts" ref="line"></div>
  </div>
</template>

<script setup lang="ts">
defineOptions({
  name: 'LinePage',
})
import * as echarts from 'echarts'
import { ref, onMounted } from 'vue'
//获取图形图标的节点
let line = ref()
onMounted(() => {
  let mycharts = echarts.init(line.value)
  //设置配置项
  mycharts.setOption({
    //标题组件
    title: {
      text: '访问量',
      textStyle: {
        color: '#fff', // 适配深色背景
      },
    },
    //提示框组件 - 新增交互效果
    tooltip: {
      trigger: 'axis',
      backgroundColor: 'rgba(0, 0, 0, 0.7)',
      textStyle: { color: '#fff' },
    },
    //x|y轴
    xAxis: {
      type: 'category',
      //两侧不留白
      boundaryGap: false,
      //分割线不要
      splitLine: {
        show: false,
      },
      data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日'],
      //轴线的设置
      axisLine: {
        show: true,
        lineStyle: { color: 'rgba(255, 255, 255, 0.3)' },
      },
      //刻度
      axisTick: {
        show: true,
        lineStyle: { color: 'rgba(255, 255, 255, 0.3)' },
      },
      axisLabel: {
        color: 'rgba(255, 255, 255, 0.8)',
      },
    },
    yAxis: {
      splitLine: {
        show: true,
        lineStyle: { color: 'rgba(255, 255, 255, 0.1)' },
      },
      //轴线的设置
      axisLine: {
        show: true,
        lineStyle: { color: 'rgba(255, 255, 255, 0.3)' },
      },
      //刻度
      axisTick: {
        show: true,
        lineStyle: { color: 'rgba(255, 255, 255, 0.3)' },
      },
      axisLabel: {
        color: 'rgba(255, 255, 255, 0.8)',
      },
    },
    grid: {
      left: 40,
      top: 40,
      right: 20,
      bottom: 20,
    },
    //系列 - 保持单条波浪线并强化填充效果
    series: [
      {
        type: 'line',
        data: [120, 1240, 66, 2299, 321, 890, 1200],
        //强化平滑效果，实现波浪感
        smooth: true,
        //增加曲线张力，让波浪更明显
        symbol: 'none', // 隐藏数据点，突出波浪线
        lineStyle: {
          width: 3, // 线条加粗
          color: 'rgba(255, 215, 0, 0.8)', // 金色线条
        },
        //增强填充效果，使用渐变透明度
        areaStyle: {
          color: {
            type: 'linear',
            x: 0,
            y: 0,
            x2: 0,
            y2: 1,
            colorStops: [
              {
                offset: 0,
                color: 'rgba(255, 215, 0, 0.5)', // 顶部半透明黄色
              },
              {
                offset: 1,
                color: 'rgba(255, 215, 0, 0.05)', // 底部几乎透明
              },
            ],
            global: false,
          },
        },
        //添加阴影效果增强立体感
        emphasis: {
          scale: false,
        },
      },
    ],
  })

  // 新增窗口大小自适应
  window.addEventListener('resize', () => {
    mycharts.resize()
  })
})
</script>

<style scoped lang="scss">
.box5 {
  width: 100%;
  height: 100%;
  background: url(../../images/dataScreen-main-cb.png) no-repeat;
  background-size: 100% 100%;
  margin: 0 20px 20px 20px;

  .title {
    margin-left: 10px;

    p {
      color: white;
      font-size: 20px;
      margin: 10px 0; // 微调边距
    }
  }

  .charts {
    height: calc(100% - 40px);
  }
}
</style>
