<template>
  <div class="box4" ref="map">我是地图组件</div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import * as echarts from 'echarts'
//引入中国地图的JSON数据
import chinaJSON from './china.json'
//获取DOM元素
let map = ref()
//注册中国地图
echarts.registerMap('china', chinaJSON as any)
onMounted(() => {
  let mychart = echarts.init(map.value)
  //设置配置项
  mychart.setOption({
    // 背景色设为透明，更好融入深蓝科技感背景
    backgroundColor: 'transparent',

    //地图组件
    geo: {
      map: 'china', //中国地图
      roam: true, //鼠标缩放的效果
      //地图的位置调试
      left: 72,
      top: 66,
      right: 72,
      zoom: 1.2,
      bottom: 0,
      //地图上的文字的设置
      label: {
        show: true, //文字显示出来
        color: 'rgba(140, 200, 255, 0.9)', // 淡蓝色文字
        fontSize: 12,
        shadowBlur: 5,
        shadowColor: 'rgba(0, 150, 255, 0.3)',
      },

      itemStyle: {
        color: {
          type: 'linear',
          x: 0,
          y: 0,
          x2: 0,
          y2: 1,
          colorStops: [
            {
              offset: 0,
              color: 'rgba(44, 130, 255, 0.8)', // 亮蓝色起点
            },
            {
              offset: 1,
              color: 'rgba(15, 70, 180, 0.8)', // 深蓝色终点
            },
          ],
          global: false,
        },
        opacity: 0.5, // 提高不透明度增强视觉冲击
        borderColor: 'rgba(100, 180, 255, 0.6)', // 亮蓝色边框增强科技感
        borderWidth: 1.5,
        shadowBlur: 10,
        shadowColor: 'rgba(50, 150, 255, 0.4)', // 光晕增强科技感
      },
      //地图高亮的效果
      emphasis: {
        itemStyle: {
          color: 'rgba(15, 40, 90, 0.8)', // 高亮时保持深色
          borderColor: 'rgba(0, 180, 255, 0.5)',
          borderWidth: 1.5,
        },
        label: {
          color: 'rgba(170, 220, 255, 1)',
          fontSize: 14,
          shadowBlur: 10,
          shadowColor: 'rgba(0, 180, 255, 0.4)',
        },
      },
    },
    //布局位置
    grid: {
      left: 0,
      top: 0,
      right: 0,
      bottom: 0,
    },
    series: [
      {
        type: 'lines', //航线的系列
        zlevel: 2,
        data: [
          {
            coords: [
              [116.405285, 39.904989],
              [119.306239, 26.075302],
            ], // 北京-福州
            lineStyle: {
              color: 'rgba(0, 200, 255, 0.5)', // 降低饱和度
              width: 1.8,
              curveness: 0.2,
              shadowBlur: 0, // 取消高亮光晕
            },
          },
          {
            coords: [
              [116.405285, 39.904989],
              [114.298572, 30.584355],
            ], // 北京-武汉
            lineStyle: {
              color: 'rgba(200, 150, 255, 0.5)',
              width: 1.8,
              curveness: 0.15,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [121.473701, 31.230416],
              [113.264385, 23.129113],
            ], // 上海-广州
            lineStyle: {
              color: 'rgba(150, 200, 255, 0.5)',
              width: 1.8,
              curveness: 0.2,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [113.264385, 23.129113],
              [106.550464, 29.563009],
            ], // 广州-重庆
            lineStyle: {
              color: 'rgba(100, 200, 255, 0.5)',
              width: 1.8,
              curveness: 0.2,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [104.066541, 30.572269],
              [120.15507, 30.274084],
            ], // 成都-杭州
            lineStyle: {
              color: 'rgba(180, 200, 255, 0.5)',
              width: 1.8,
              curveness: 0.25,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [121.473701, 31.230416],
              [117.200983, 39.084158],
            ], // 上海-天津
            lineStyle: {
              color: 'rgba(0, 200, 255, 0.5)',
              width: 1.8,
              curveness: 0.1,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [91.132212, 29.660361],
              [116.405285, 39.904989],
            ], // 拉萨-北京
            lineStyle: {
              color: 'rgba(200, 150, 255, 0.5)',
              width: 1.8,
              curveness: 0.3,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [110.346656, 20.025785],
              [121.509062, 31.224361],
            ], // 海口-上海
            lineStyle: {
              color: 'rgba(150, 200, 255, 0.5)',
              width: 1.8,
              curveness: 0.2,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [123.429096, 41.805554],
              [113.264385, 23.129113],
            ], // 沈阳-广州
            lineStyle: {
              color: 'rgba(100, 200, 255, 0.5)',
              width: 1.8,
              curveness: 0.25,
              shadowBlur: 0,
            },
          },
          {
            coords: [
              [87.616888, 43.825592],
              [103.823557, 36.058039],
            ], // 乌鲁木齐-兰州
            lineStyle: {
              color: 'rgba(180, 200, 255, 0.5)',
              width: 1.8,
              curveness: 0.2,
              shadowBlur: 0,
            },
          },
        ],
        //开启动画特效
        effect: {
          show: true,
          symbol: 'circle',
          symbolSize: 5,
          trailLength: 0.6, // 缩短尾迹
          color: 'rgba(255, 255, 255, 0.7)', // 淡化动画颜色
          loop: true,
          period: 4, // 放慢动画
        },
        // 线动画
        lineStyle: {
          width: 1.8,
          opacity: 0.5,
          curveness: 0.2,
        },
      },
    ],
  })

  // 监听窗口大小变化，重绘图表
  window.addEventListener('resize', () => {
    mychart.resize()
  })
})
</script>

<style scoped>
.box4 {
  width: 100%;
  height: 100%;
}
</style>
