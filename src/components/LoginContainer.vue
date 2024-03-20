<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink, useRouter } from 'vue-router'
import axios from 'axios'

const router = useRouter()

const loginInfo = ref({
  email: '',
  password: ''
})

const login = () => {
  console.log(loginInfo.value)

  axios
    .post('http://localhost:8000/users/login/', loginInfo.value)
    .then((res) => {
      localStorage.setItem('token', res.data.access)
      router.push({ path: '/' })
    })
    .catch((err) => {
      console.log(err)
      alert('Email ou senha incorretos')
    })
}
</script>

<template>
  <div class="login-container">
    <div>
      <img src="/fin-logo.png" alt="logo" />
    </div>
    <div class="login-form">
      <section>
        <input type="email" v-model="loginInfo.email" placeholder="Email" />
        <input type="password" v-model="loginInfo.password" placeholder="Senha" />
      </section>
      <section>
        <RouterLink to="/register">Crie aqui uma conta</RouterLink>
        <button @click="login">Login</button>
      </section>
    </div>
  </div>
</template>

<style>
.login-container {
  background-color: #f7f7f7;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 55vh;
  width: 80vw;
  border: 4px solid #9963f7;
  border-radius: 1rem;

  padding: 4%;

  div:nth-child(1) {
    width: 100%;
    height: 35%;
    display: flex;
    justify-content: center;

    img {
      height: 100%;
    }
  }

  .login-form {
    height: 60%;
    width: 100%;

    display: flex;
    flex-direction: column;
    justify-content: space-between;

    input {
      width: 100%;
      height: 3rem;
      padding: 0.5rem;
      border-radius: 0.5rem;
      border: 1px solid #9963f7;
    }

    section:nth-child(1) {
      height: 50%;
      display: flex;
      flex-direction: column;
      gap: 0.4rem;
    }

    section:nth-child(2) {
      height: 50%;
      display: flex;
      flex-direction: column;
      gap: 0.8rem;
      align-items: center;
      margin-top: 1rem;

      button {
        width: 100%;
        height: 3rem;
        border: none;
        border-radius: 0.5rem;
        background-color: #9963f7;
        color: white;
        font-size: 1.2rem;
        cursor: pointer;
      }

      button:hover {
        background-color: #ebd6ff;
        color: #9963f7;
        transition: 0.25s;
      }

      button:not(:hover) {
        transition: 0.2s;
      }
    }
  }

  @media (min-width: 600px) {
    width: 50vw;
    height: 40vh;
  }

  @media (min-width: 1366px) {
    width: 35vw;
    height: 55vh;
  }

  @media (min-width: 1920) {
    width: 30vw;
  }
}
</style>
