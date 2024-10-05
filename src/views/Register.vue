<template>
    <div class="register">
      <h2>注册</h2>
      <form @submit.prevent="register">
        <div>
          <label for="username">用户名：</label>
          <input type="text" id="username" v-model="username" required>
        </div>
        <div>
          <label for="password">密码：</label>
          <input type="password" id="password" v-model="password" required>
        </div>
        <div>
          <label for="confirmPassword">确认密码：</label>
          <input type="password" id="confirmPassword" v-model="confirmPassword" required>
        </div>
        <button type="submit">注册</button>
      </form>
      <p>已有账号？<router-link to="/login">立即登录</router-link></p>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  
  export default {
    name: 'Register',
    setup() {
      const username = ref('');
      const password = ref('');
      const confirmPassword = ref('');
      const router = useRouter();
  
      const register = async () => {
        if (password.value !== confirmPassword.value) {
          alert('两次输入的密码不一致');
          return;
        }
        try {
          const response = await fetch(`http://localhost:8080/api/users/register?username=${username.value}&password=${password.value}`, {
            method: 'POST',
          });
          if (response.ok) {
            const data = await response.json();
            console.log('注册成功:', data);
            alert('注册成功，请登录');
            router.push('/login');
          } else {
            alert('注册失败，请重试');
          }
        } catch (error) {
          console.error('注册错误:', error);
          alert('注册出错，请稍后重试');
        }
      };
  
      return {
        username,
        password,
        confirmPassword,
        register
      };
    }
  };
  </script>
  
  <style scoped>
  .register {
    max-width: 300px;
    margin: 0 auto;
    padding: 20px;
  }
  form div {
    margin-bottom: 10px;
  }
  </style>