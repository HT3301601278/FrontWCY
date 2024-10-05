<template>
    <div class="device-detail">
      <h1>{{ device.name }} 详情</h1>
      <el-descriptions title="设备信息" :column="1" border>
        <el-descriptions-item label="设备ID">{{ device.id }}</el-descriptions-item>
        <el-descriptions-item label="创建时间">{{ device.createdAt }}</el-descriptions-item>
      </el-descriptions>

      <div class="real-time-data">
        <h2>实时数据</h2>
        <el-statistic title="当前光照强度" :value="currentLightIntensity" suffix="lux">
          <template #prefix>
            <el-icon><Sunny /></el-icon>
          </template>
        </el-statistic>
      </div>

      <div class="add-data-form">
        <h2>添加新数据</h2>
        <el-form @submit.prevent="addDeviceData" :inline="true">
          <el-form-item label="光照强度">
            <el-input-number v-model="newDataValue" :min="0" :max="100000" placeholder="请输入光照强度"></el-input-number>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" native-type="submit">添加数据</el-button>
          </el-form-item>
        </el-form>
      </div>

      <div class="data-history">
        <h2>历史数据</h2>
        <el-table :data="deviceData" style="width: 100%">
          <el-table-column prop="value" label="光照强度 (lux)" sortable></el-table-column>
          <el-table-column prop="timestamp" label="时间戳" sortable></el-table-column>
        </el-table>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted, computed } from 'vue';
  import { useRoute } from 'vue-router';
  import { ElMessage } from 'element-plus';
  import { Sunny } from '@element-plus/icons-vue';

  export default {
    name: 'DeviceDetail',
    components: { Sunny },
    setup() {
      const route = useRoute();
      const device = ref({});
      const currentLightIntensity = ref(0);
      const newDataValue = ref('');
      const deviceData = ref([]);

      const fetchDeviceDetails = async (deviceId) => {
        try {
          const response = await fetch(`http://localhost:8080/api/devices/${deviceId}`);
          if (response.ok) {
            device.value = await response.json();
          } else {
            ElMessage.error('获取设备详情失败');
          }
        } catch (error) {
          console.error('获取设备详情错误:', error);
          ElMessage.error('获取设备详情出错，请稍后重试');
        }
      };

      const fetchDeviceData = async (deviceId) => {
        try {
          const response = await fetch(`http://localhost:8080/api/device-data/device/${deviceId}`);
          if (response.ok) {
            deviceData.value = await response.json();
            if (deviceData.value.length > 0) {
              currentLightIntensity.value = deviceData.value[0].value;
            }
          } else {
            ElMessage.error('获取设备数据失败');
          }
        } catch (error) {
          console.error('获取设备数据错误:', error);
          ElMessage.error('获取设备数据出错，请稍后重试');
        }
      };

      const addDeviceData = async () => {
        try {
          const response = await fetch(`http://localhost:8080/api/device-data?deviceId=${device.value.id}&value=${newDataValue.value}`, {
            method: 'POST',
          });
          if (response.ok) {
            const data = await response.json();
            deviceData.value.unshift(data);
            currentLightIntensity.value = data.value;
            ElMessage.success('数据添加成功');
            newDataValue.value = '';
          } else {
            ElMessage.error('添加数据失败');
          }
        } catch (error) {
          console.error('添加数据错误:', error);
          ElMessage.error('添加数据出错，请稍后重试');
        }
      };

      onMounted(async () => {
        const deviceId = route.params.id;
        await fetchDeviceDetails(deviceId);
        await fetchDeviceData(deviceId);
      });

      return {
        device,
        currentLightIntensity,
        newDataValue,
        deviceData,
        addDeviceData
      };
    }
  };
  </script>
  
  <style scoped>
  .device-detail {
    padding: 20px;
  }
  .real-time-data, .data-chart, .add-data-form, .data-history {
    margin-top: 30px;
  }
  h1, h2 {
    margin-bottom: 20px;
  }
  </style>