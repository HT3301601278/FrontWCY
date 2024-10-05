<template>
    <div class="login-container">
      <el-card class="login-card">
        <template #header>
          <h2 class="login-title">登录</h2>
        </template>
        <el-form @submit.prevent="login" class="login-form">
          <el-form-item>
            <el-input v-model="username" placeholder="用户名" prefix-icon="el-icon-user"></el-input>
          </el-form-item>
          <el-form-item>
            <el-input v-model="password" type="password" placeholder="密码" prefix-icon="el-icon-lock"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" native-type="submit" class="login-button">登录</el-button>
          </el-form-item>
        </el-form>
        <div class="register-link">
          <span>还没有账号？</span>
          <el-link type="primary" @click="$router.push('/register')">立即注册</el-link>
        </div>
      </el-card>
    </div>
</template>

<script>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { ElMessage } from 'element-plus';

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
          const errorData = await response.json();
          ElMessage.error(errorData.message || '登录失败，请检查用户名和密码');
        }
      } catch (error) {
        console.error('登录错误:', error);
        ElMessage.error('登录出错，请稍后重试');
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
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f2f5;
}

.login-card {
  width: 350px;
}

.login-title {
  text-align: center;
  color: #409EFF;
  margin: 0;
}

.login-form {
  margin-top: 20px;
}

.login-button {
  width: 100%;
}

.register-link {
  display: flex;
  align-items: center;
  justify-content: center;
}

.register-link span {
  margin-right: 5px;
}
</style>