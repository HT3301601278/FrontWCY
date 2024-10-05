<template>
  <div class="dashboard">
    <el-row :gutter="20">
      <el-col :span="24">
        <h1 class="dashboard-title">农作物光强在线监测系统</h1>
      </el-col>
    </el-row>

    <el-row :gutter="20">
      <el-col :span="8">
        <el-card class="stat-card" :body-style="{ padding: '0px' }">
          <div class="stat-icon">
            <el-icon :size="40" color="#E6A23C">
              <Sunny />
            </el-icon>
          </div>
          <div class="stat-content">
            <div class="stat-value">{{ averageLightIntensity }} lux</div>
            <div class="stat-label">平均光照强度</div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="8">
        <el-card class="stat-card" :body-style="{ padding: '0px' }">
          <div class="stat-icon">
            <el-icon :size="40" color="#67C23A">
              <Sunrise />
            </el-icon>
          </div>
          <div class="stat-content">
            <div class="stat-value">{{ maxLightIntensity }} lux</div>
            <div class="stat-label">最高光照强度</div>
          </div>
        </el-card>
      </el-col>
      <el-col :span="8">
        <el-card class="stat-card" :body-style="{ padding: '0px' }">
          <div class="stat-icon">
            <el-icon :size="40" color="#909399">
              <Sunset />
            </el-icon>
          </div>
          <div class="stat-content">
            <div class="stat-value">{{ minLightIntensity }} lux</div>
            <div class="stat-label">最低光照强度</div>
          </div>
        </el-card>
      </el-col>
    </el-row>

    <el-row :gutter="20" class="chart-row">
      <el-col :span="24">
        <el-card class="chart-card">
          <div class="chart-container" id="lightIntensityChart"></div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { Sunny, Sunrise, Sunset } from '@element-plus/icons-vue';
import * as echarts from 'echarts';

export default {
  name: 'Dashboard',
  components: { Sunny, Sunrise, Sunset },
  setup() {
    const averageLightIntensity = ref(0);
    const maxLightIntensity = ref(0);
    const minLightIntensity = ref(0);
    const deviceData = ref([]);

    const fetchDeviceData = async () => {
      try {
        const response = await fetch('http://localhost:8080/api/device-data/device/1');
        if (response.ok) {
          deviceData.value = await response.json();
          calculateStatistics();
          renderChart();
        } else {
          console.error('获取设备数据失败');
        }
      } catch (error) {
        console.error('获取设备数据错误:', error);
      }
    };

    const calculateStatistics = () => {
      const values = deviceData.value.map(item => item.value);
      averageLightIntensity.value = (values.reduce((a, b) => a + b, 0) / values.length).toFixed(2);
      maxLightIntensity.value = Math.max(...values).toFixed(2);
      minLightIntensity.value = Math.min(...values).toFixed(2);
    };

    const renderChart = () => {
      const chartDom = document.getElementById('lightIntensityChart');
      const myChart = echarts.init(chartDom);

      // 按小时对数据进行分组和处理
      const groupedData = Array(24).fill().map(() => []);
      deviceData.value.forEach(item => {
        const date = new Date(item.timestamp);
        const hour = date.getHours();
        groupedData[hour].push(item);
      });

      // 每个小时随机选择一个数据点
      const processedData = groupedData.map((hourData, hour) => {
        if (hourData.length > 0) {
          const randomIndex = Math.floor(Math.random() * hourData.length);
          return [hour, hourData[randomIndex].value];
        }
        return null;
      }).filter(item => item !== null);

      const option = {
        title: {
          text: '24小时光照强度变化'
        },
        tooltip: {
          trigger: 'axis',
          formatter: function(params) {
            const hour = params[0].value[0];
            const value = params[0].value[1];
            return `${hour}:00 - ${(hour + 1) % 24}:00<br/>光照强度: ${value} lux`;
          }
        },
        xAxis: {
          type: 'category',
          data: Array.from({length: 24}, (_, i) => i),
          axisLabel: {
            formatter: (value) => `${value}:00`
          },
          axisTick: {
            alignWithLabel: true
          },
          axisLine: {
            onZero: false
          },
          splitLine: {
            show: true
          }
        },
        yAxis: {
          type: 'value',
          name: '光照强度 (lux)'
        },
        series: [{
          data: processedData,
          type: 'line',
          smooth: true,
          symbolSize: 8,
          itemStyle: {
            color: '#67C23A'
          },
          lineStyle: {
            color: '#67C23A'
          }
        }]
      };

      myChart.setOption(option);
    };

    onMounted(() => {
      fetchDeviceData();
    });

    return {
      averageLightIntensity,
      maxLightIntensity,
      minLightIntensity,
      deviceData
    };
  }
};
</script>

<style scoped>
.dashboard {
  padding: 20px;
}

.dashboard-title {
  text-align: center;
  color: #303133;
  margin-bottom: 30px;
}

.stat-card {
  height: 120px;
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.stat-icon {
  width: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.stat-content {
  flex-grow: 1;
  text-align: center;
}

.stat-value {
  font-size: 24px;
  font-weight: bold;
  color: #303133;
}

.stat-label {
  font-size: 14px;
  color: #909399;
}

.chart-row {
  margin-top: 20px;
}

.chart-card, .device-status-card {
  height: 400px;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chart-container {
  height: 400px;
  width: 100%;
}

.chart-placeholder {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f5f7fa;
  color: #909399;
}
</style>
