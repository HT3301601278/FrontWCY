<template>
    <div class="devices">
      <h1>光照传感器管理</h1>
      <el-button type="primary" @click="showAddDeviceForm = true">添加新传感器</el-button>
      
      <el-dialog v-model="showAddDeviceForm" title="添加新传感器">
        <el-form @submit.prevent="addDevice">
          <el-form-item label="传感器名称">
            <el-input v-model="newDeviceName" placeholder="请输入传感器名称" required></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" native-type="submit">添加</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>

      <el-table :data="devices" style="width: 100%; margin-top: 20px;">
        <el-table-column prop="id" label="ID" width="80"></el-table-column>
        <el-table-column prop="name" label="名称"></el-table-column>
        <el-table-column prop="createdAt" label="创建时间"></el-table-column>
        <el-table-column label="操作" width="180">
          <template #default="scope">
            <el-button size="small" @click="viewDeviceDetails(scope.row.id)">查看详情</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { ElMessage } from 'element-plus';
import { useRouter } from 'vue-router';

export default {
  name: 'Devices',
  setup() {
    const devices = ref([]);
    const showAddDeviceForm = ref(false);
    const newDeviceName = ref('');
    const router = useRouter();

    const fetchDevices = async () => {
      try {
        const response = await fetch('http://localhost:8080/api/devices');
        if (response.ok) {
          devices.value = await response.json();
        } else {
          ElMessage.error('获取设备列表失败');
        }
      } catch (error) {
        console.error('获取设备列表错误:', error);
        ElMessage.error('获取设备列表出错');
      }
    };

    const addDevice = async () => {
      try {
        const response = await fetch(`http://localhost:8080/api/devices?name=${newDeviceName.value}`, {
          method: 'POST',
        });
        if (response.ok) {
          const newDevice = await response.json();
          devices.value.push(newDevice);
          showAddDeviceForm.value = false;
          newDeviceName.value = '';
          ElMessage.success('添加设备成功');
        } else {
          ElMessage.error('添加设备失败');
        }
      } catch (error) {
        console.error('添加设备错误:', error);
        ElMessage.error('添加设备出错');
      }
    };

    const viewDeviceDetails = (id) => {
      router.push(`/device/${id}`);
    };

    onMounted(fetchDevices);

    return {
      devices,
      showAddDeviceForm,
      newDeviceName,
      addDevice,
      viewDeviceDetails
    };
  }
};
</script>

<style scoped>
.devices {
  padding: 20px;
}
</style>