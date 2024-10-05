<template>
    <div class="device-card" :class="{ 'offline': device.status === 'offline' }">
      <h3>{{ device.name }}</h3>
      <p>状态: {{ device.status === 'online' ? '在线' : '离线' }}</p>
      <p>光照强度: {{ device.lightIntensity }} lux</p>
      <router-link :to="`/device/${device.id}`">查看详情</router-link>
    </div>
  </template>
  
  <script>
  export default {
    name: 'DeviceCard',
    props: {
      device: {
        type: Object,
        required: true
      }
    }
  };
  </script>
  
  <style scoped>
  .device-card {
    border: 1px solid #ddd;
    padding: 10px;
    border-radius: 5px;
    width: 200px;
  }
  .offline {
    opacity: 0.5;
  }
  </style>