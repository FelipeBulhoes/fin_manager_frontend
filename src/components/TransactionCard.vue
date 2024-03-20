<script setup lang="ts">
import axios from 'axios'
import { defineEmits } from 'vue'

const { transaction } = defineProps(['transaction'])

const emit = defineEmits(['transactionDeleted'])

const formatDate = (dateString: string) => {
  const date = new Date(dateString)
  const day = date.getDate().toString().padStart(2, '0')
  const month = (date.getMonth() + 1).toString().padStart(2, '0')
  const year = date.getFullYear()

  return `${day}/${month}/${year}`
}

const deleteTransaction = async () => {
  await axios
    .delete(`http://localhost:8000/transactions/delete/${transaction.id}/`, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`
      }
    })
    .then(() => {
      emit('transactionDeleted', transaction.id)
    })
    .catch((err) => {
      alert('Erro ao deletar transação')
    })
}
</script>

<template>
  <li>
    <div class="card-container">
      <div class="card-header">
        <div>
          <h4>{{ transaction.title }}</h4>
          <span>-</span>
          <span :style="{ color: transaction.type === 'income' ? 'green' : 'red' }"
            >R$ {{ transaction.amount }}</span
          >
          <span>-</span>
          <span v-if="transaction.periodicity === 'monthly'">Mensal</span>
          <span v-else-if="transaction.periodicity === 'yearly'">Anual</span>
        </div>
        <div>
          <span>{{ formatDate(transaction.created_at) }}</span>
        </div>
      </div>
      <div class="card-description">
        <p>Descrição: {{ transaction.description }}</p>
        <button @click="deleteTransaction"><img src="/deletar-lixeira.png" /></button>
      </div>
    </div>
  </li>
</template>

<style>
li {
  display: flex;
  background-color: #f7f7f7;
  border: 2px solid #9963f7;
  border-radius: 0.75rem;
  color: black;

  margin-bottom: 0.75rem;
  padding: 0.4rem 0.8rem;
}

.card-container {
  width: 100%;
  display: flex;
  flex-direction: column;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  div {
    display: flex;
    gap: 0.5rem;

    h4 {
      color: #9963f7;
      font-weight: 700;
    }
  }
}

.card-description {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: end;

  button {
    height: 1.8rem;
    display: flex;
    align-items: center;
    padding: 0.5rem 1rem;
    border: 1px solid red;
    border-radius: 5px;
    background-color: #f1f1f1;
    cursor: pointer;

    img {
      height: 16px;
    }
  }

  button:hover {
    box-shadow: 0 0 5px 0 red;
    transition: 0.15s;
  }
  button:not(:hover) {
    transition: 0.3s;
  }
}
</style>
