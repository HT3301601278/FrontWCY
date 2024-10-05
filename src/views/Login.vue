<template>
    <div class="login">
      <h2>登录</h2>
      <form @submit.prevent="login">
        <div>
          <label for="username">用户名：</label>
          <input type="text" id="username" v-model="username" required>
        </div>
        <div>
          <label for="password">密码：</label>
          <input type="password" id="password" v-model="password" required>
        </div>
        <button type="submit">登录</button>
      </form>
      <p>还没有账号？<router-link to="/register">立即注册</router-link></p>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  
  export default {
    name: 'Login',
    setup() {
      const username = ref('');
      const password = ref('');
      const router = useRouter();
  
      const login = async () => {
        try {
          const response = await fetch(`http://localhost:8080/api/users/login?username=${username.value}&password=${password.value}`, {
            method: 'POST',
          });
          if (response.ok) {
            const data = await response.json();
            console.log('登录成功:', data);
            localStorage.setItem('user', JSON.stringify(data));
            router.push('/dashboard');
          } else {
            alert('登录失败，请检查用户名和密码');
          }
        } catch (error) {
          console.error('登录错误:', error);
          alert('登录出错，请稍后重试');
        }
      };
  
      return {
        username,
        password,
        login
      };
    }
  };
  </script>
  
  <style scoped>
  .login {
    max-width: 300px;
    margin: 0 auto;
    padding: 20px;
  }
  form div {
    margin-bottom: 10px;
  }
  </style>