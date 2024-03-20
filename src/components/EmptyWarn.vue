<script setup lang="ts"></script>

<template>
  <div class="empty-list">
    <img src="/empty.png" alt="" />
    <span>Ainda não há transações</span>
  </div>
</template>

<style>
.empty-list {
  display: flex;
  background-color: #e3e3e3;

  border-radius: 1rem;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 4rem;

  span {
    color: #9963f7;
  }

  img {
    height: 1.5rem;
    margin-right: 1rem;
  }
}
</style>
