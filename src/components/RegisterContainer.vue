<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import axios from 'axios'

const registerInfo = ref({
  username: '',
  email: '',
  password: ''
})

const register = () => {
  console.log(registerInfo.value)

  axios
    .post('http://localhost:8000/users/create/', registerInfo.value)
    .then(() => {
      alert('Usuário criado com sucesso!')
    })
    .catch((err) => {
      alert('Erro ao criar usuário')
      console.log(err)
    })
  // colocar icone de certinho no botao do loading
}
</script>

<template>
  <div class="register-container">
    <div>
      <img src="/fin-logo.png" alt="logo" />
    </div>
    <div class="register-form">
      <section>
        <input type="username" v-model="registerInfo.username" placeholder="Nome" />
        <input type="email" v-model="registerInfo.email" placeholder="Email" />
        <input type="password" v-model="registerInfo.password" placeholder="Senha" />
      </section>
      <section>
        <RouterLink to="/login">Já possuí uma conta? Faça login</RouterLink>
        <button @click="register">Criar conta</button>
      </section>
    </div>
  </div>
</template>

<style>
.register-container {
  background-color: #f7f7f7;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 65vh;
  width: 80vw;
  border: 4px solid #9963f7;
  border-radius: 1rem;

  padding: 4%;

  div:nth-child(1) {
    width: 100%;
    height: 30%;
    display: flex;
    justify-content: center;

    img {
      height: 100%;
    }
  }

  .register-form {
    height: 65%;
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
    height: 50vh;
  }

  @media (min-width: 1366px) {
    width: 35vw;
    height: 65vh;
  }

  @media (min-width: 1920) {
    width: 30vw;
  }
}
</style>
