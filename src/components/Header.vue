<script setup lang="ts">
const Logoff = () => {
  localStorage.removeItem('token')
  window.location.href = '/login'
}
</script>

<template>
  <header>
    <div>
      <img src="/fin-logo.png" alt="logo" class="header-logo" />
    </div>
    <div>
      <button @click="Logoff">Logout</button>
    </div>
  </header>
</template>

<style>
header {
  position: fixed;
  width: 100%;

  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 10vh;
  padding: 0 7.5vw;
  margin-bottom: 1vh;
  background-color: #f1f1f1;
}
.header-logo {
  height: 4rem;
}
button {
  padding: 0.5rem 1rem;
  border: 2px solid #c2c2c2;
  border-radius: 5px;
  background-color: #f1f1f1;
  cursor: pointer;
}
button:hover {
  border: 2px solid #9963f7;
  transition: 0.3s;
}

@media (min-width: 1366px) {
  header {
    padding: 0 11.5vw;
  }
}

@media (min-width: 1600px) {
  header {
    padding: 0 15.5vw;
  }
}

@media (min-width: 1921px) {
  header {
    padding: 0 21vw;
  }
}
</style>
