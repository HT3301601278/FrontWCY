<template>
  <div id="app">
    <nav v-if="isLoggedIn && !isLoginPage">
      <router-link to="/dashboard">仪表盘</router-link> |
      <router-link to="/devices">光照传感器管理</router-link> |
      <a href="#" @click.prevent="logout">退出登录</a>
    </nav>
    <router-view/>
  </div>
</template>

<script>
import { computed } from 'vue';
import { useRouter, useRoute } from 'vue-router';

export default {
  name: 'App',
  setup() {
    const router = useRouter();
    const route = useRoute();

    const isLoggedIn = computed(() => {
      const user = localStorage.getItem('user');
      console.log('Current user in localStorage:', user);
      return user !== null && user !== 'undefined';
    });

    const isLoginPage = computed(() => {
      return route.path === '/login' || route.path === '/register';
    });

    console.log('Is user logged in?', isLoggedIn.value);

    const logout = () => {
      localStorage.removeItem('user');
      router.push('/login');
    };

    return {
      isLoggedIn,
      isLoginPage,
      logout
    };
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
