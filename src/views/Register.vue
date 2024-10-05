<template>
    <div class="register-container">
      <el-card class="register-card">
        <template #header>
          <h2 class="register-title">注册</h2>
        </template>
        <el-form @submit.prevent="register" class="register-form">
          <el-form-item>
            <el-input v-model="username" placeholder="用户名" prefix-icon="el-icon-user"></el-input>
          </el-form-item>
          <el-form-item>
            <el-input v-model="password" type="password" placeholder="密码" prefix-icon="el-icon-lock"></el-input>
          </el-form-item>
          <el-form-item>
            <el-input v-model="confirmPassword" type="password" placeholder="确认密码" prefix-icon="el-icon-lock"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" native-type="submit" class="register-button">注册</el-button>
          </el-form-item>
        </el-form>
        <div class="login-link">
          <span>已有账号？</span>
          <el-link type="primary" @click="$router.push('/login')">立即登录</el-link>
        </div>
      </el-card>
    </div>
</template>

<script>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { ElMessage } from 'element-plus';

export default {
  name: 'Register',
  setup() {
    const username = ref('');
    const password = ref('');
    const confirmPassword = ref('');
    const router = useRouter();

    const register = async () => {
      if (password.value !== confirmPassword.value) {
        ElMessage.error('两次输入的密码不一致');
        return;
      }
      try {
        const response = await fetch(`http://localhost:8080/api/users/register?username=${username.value}&password=${password.value}`, {
          method: 'POST',
        });
        if (response.ok) {
          const data = await response.json();
          console.log('注册成功:', data);
          ElMessage.success('注册成功，请登录');
          router.push('/login');
        } else {
          const errorData = await response.json();
          ElMessage.error(errorData.message || '注册失败，请重试');
        }
      } catch (error) {
        console.error('注册错误:', error);
        ElMessage.error('注册出错，请稍后重试');
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
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f2f5;
}

.register-card {
  width: 350px;
}

.register-title {
  text-align: center;
  color: #409EFF;
  margin: 0;
}

.register-form {
  margin-top: 20px;
}

.register-button {
  width: 100%;
}

.login-link {
  display: flex;
  align-items: center;
  justify-content: center;
}

.login-link span {
  margin-right: 5px;
}
</style>