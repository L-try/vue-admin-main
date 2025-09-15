<template>
  <div class="box6">
    <!-- 标题区域：包含标题文本和装饰图标 -->
    <div class="title">
      <p>热门景区排行</p>
      <img src="../../images/dataScreen-title.png" alt="标题装饰图标" />
    </div>
    <!-- 图表容器：用于渲染ECharts图表 -->
    <div class="charts" ref="charts"></div>
  </div>
</template>

<script setup lang="ts">
// 引入ECharts库用于数据可视化
import * as echarts from 'echarts'
// 引入Vue的ref用于获取DOM元素，onMounted用于组件挂载后执行初始化
import { ref, onMounted } from 'vue'

// 获取图表容器的DOM引用
let charts = ref()

// 组件挂载完成后初始化图表（确保DOM已渲染）
onMounted(() => {
  // 初始化ECharts实例，绑定到图表容器
  let mychart = echarts.init(charts.value)

  // 设置图表配置项（核心配置）
  mychart.setOption({
    // 标题组件配置
    title: {
      text: '景区排行', // 主标题文本
      link: 'http://www.baidu.com', // 标题点击跳转链接
      left: '50%', // 水平居中显示
      top: 10, // 距离顶部10px
      textAlign: 'center', // 文本居中对齐
      textStyle: {
        color: '#f0f0f0', // 主标题颜色（浅灰更适配深色背景）
        fontSize: 20,
        fontWeight: 'bold', // 加粗突出标题
      },
      subtext: '各大景区热度排行', // 子标题文本
      subtextStyle: {
        color: '#b3b3b3', // 子标题颜色（稍浅于主标题）
        fontSize: 14,
        padding: [5, 0, 0, 0], // 顶部留白增加层次感
      },
    },
    // X轴配置
    xAxis: {
      type: 'category', // 类目轴（适用于离散数据）
      data: ['景区A', '景区B', '景区C', '景区D', '景区E', '景区F', '景区G'], // 补充景区名称数据
      axisLine: {
        lineStyle: {
          color: 'rgba(255,255,255,0.3)', // X轴线颜色（半透明白色适配背景）
        },
      },
      axisLabel: {
        color: '#ccc', // 轴标签颜色
        rotate: 30, // 标签旋转30度避免重叠
        interval: 0, // 强制显示所有标签
      },
    },
    // Y轴配置
    yAxis: {
      type: 'value', // 数值轴（适用于连续数据）
      axisLine: {
        lineStyle: {
          color: 'rgba(255,255,255,0.3)', // Y轴线颜色
        },
      },
      splitLine: {
        lineStyle: {
          color: 'rgba(255,255,255,0.1)', // 网格线颜色（更淡的半透明）
        },
      },
      axisLabel: {
        color: '#ccc', // 轴标签颜色
      },
    },
    // 图表布局配置（控制图表与容器边距）
    grid: {
      left: 40, // 左内边距
      right: 20, // 右内边距
      bottom: 60, // 下内边距（预留标签空间）
      top: 70, // 上内边距（避开标题）
      containLabel: true, // 确保标签不超出容器
    },
    // 系列数据配置（决定图表展示类型和数据）
    series: [
      {
        type: 'bar', // 柱状图类型
        data: [10, 20, 30, 40, 50, 60, 70], // 数据值
        barWidth: 25, // 柱形宽度
        // 柱形上的文本标签配置
        label: {
          show: true,
          position: 'insideTop', // 标签在柱形顶部内部
          color: '#fff', // 标签颜色（白色更清晰）
          fontSize: 12,
          fontWeight: 'bold',
        },
        // 显示柱形背景
        showBackground: true,
        backgroundStyle: {
          color: 'rgba(255,255,255,0.1)', // 背景色（半透明白色）
        },
        // 柱形样式配置
        itemStyle: {
          borderRadius: [8, 8, 0, 0], // 顶部圆角，底部直角
          // 动态设置柱形颜色（渐变色更美观）
          color: function (data: any) {
            // 定义渐变色数组（适配景区主题的自然色系）
            const colors = [
              'rgba(255, 99, 132, 0.8)',
              'rgba(255, 159, 64, 0.8)',
              'rgba(255, 205, 86, 0.8)',
              'rgba(75, 192, 192, 0.8)',
              'rgba(54, 162, 235, 0.8)',
              'rgba(153, 102, 255, 0.8)',
              'rgba(201, 203, 207, 0.8)',
            ]
            return colors[data.dataIndex]
          },
        },
        // 鼠标悬停效果
        emphasis: {
          itemStyle: {
            opacity: 1, // 悬停时不透明
          },
        },
      },
      {
        type: 'line', // 折线图类型
        data: [10, 20, 30, 40, 50, 60, 90], // 折线数据
        smooth: true, // 平滑曲线
        symbol: 'circle', // 数据点样式（圆形）
        symbolSize: 8, // 数据点大小
        lineStyle: {
          width: 3, // 线宽
          color: 'rgba(255, 255, 255, 0.8)', // 线条颜色（白色半透明）
        },
        itemStyle: {
          color: '#fff', // 数据点颜色
          borderColor: 'rgba(255,255,255,0.5)', // 数据点边框
          borderWidth: 2,
        },
        // 折线填充区域
        areaStyle: {
          color: {
            type: 'linear',
            x: 0,
            y: 0,
            x2: 0,
            y2: 1,
            colorStops: [
              { offset: 0, color: 'rgba(255,255,255,0.3)' },
              { offset: 1, color: 'rgba(255,255,255,0)' },
            ],
          },
        },
      },
    ],
    // 提示框配置（鼠标悬停时显示详情）
    tooltip: {
      trigger: 'axis', // 按轴触发
      backgroundColor: 'rgba(30, 30, 30, 0.9)', // 背景色（深色半透明）
      borderColor: 'rgba(255,255,255,0.2)', // 边框色
      borderWidth: 1,
      textStyle: {
        color: '#fff', // 提示文本颜色
      },
      padding: 10, // 内边距
      formatter: function (params: any) {
        // 自定义提示内容格式
        return `${params[0].name}<br/>${params[0].seriesName}: ${params[0].value}<br/>${params[1].seriesName}: ${params[1].value}`
      },
    },
    // 图例配置（区分不同系列）
    legend: {
      data: ['景区访问量', '景区热度指数'], // 图例名称
      top: 40, // 位置在标题下方
      left: 'center',
      textStyle: {
        color: '#ccc', // 图例文字颜色
      },
      itemWidth: 12, // 图例标记宽度
      itemHeight: 12,
    },
  })

  // 监听窗口大小变化，自动调整图表尺寸
  window.addEventListener('resize', () => {
    mychart.resize()
  })
})
</script>

<style scoped lang="scss">
.box6 {
  width: 100%;
  height: 100%;
  // 保留原有背景图
  background: url(../../images/dataScreen-main-rt.png) no-repeat;
  background-size: 100% 100%;
  margin: 20px 0px;
  padding: 15px; // 增加内边距避免内容贴边

  /* 标题样式 */
  .title {
    color: rgb(247, 243, 243); /* 文字颜色 */
    font-size: 20px; /* 文字大小 */
    margin: 0 0 10px 0; /* 边距设置 */
    p {
      color: rgb(251, 249, 249); /* 文字颜色 */
      font-size: 20px; /* 文字大小 */
      margin: 0; /* 清除默认边距 */
      padding: 0; /* 上下内边距 */
    }
  }
  .charts {
    height: calc(100% - 50px); // 调整高度计算（适配标题区域高度变化）
    width: 100%; // 确保宽度充满容器
  }
}
</style>
