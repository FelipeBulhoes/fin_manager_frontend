<script setup lang="ts">
import axios from 'axios'

const emit = defineEmits(['transactionCreated'])

let amountValue: string
const createInfo = {
  title: '',
  type: '',
  amount: 0,
  periodicity: '',
  description: ''
}

const createTransaction = (event: MouseEvent) => {
  event.preventDefault()

  createInfo.amount = parseFloat(amountValue)

  axios
    .post('http://localhost:8000/transactions/create/', createInfo, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('token')}`
      }
    })
    .then((res) => {
      emit('transactionCreated', res.data)
    })
    .catch((err) => {
      alert('Erro ao criar transação. Verifique se todos os campos foram preenchidos corretamente.')
      console.log(err)
    })
}
</script>

<template>
  <div class="create-container">
    <form>
      <h2>Criar transação:</h2>
      <div>
        <input type="text" placeholder="Título" v-model="createInfo.title" />
        <select name="periodicity" v-model="createInfo.periodicity">
          <option value="" disabled selected>Recorrência:</option>
          <option value="monthly">Mensal</option>
          <option value="yearly">Anual</option>
        </select>
      </div>
      <div>
        <input type="number" placeholder="Valor" v-model="amountValue" />
        <select name="type" v-model="createInfo.type">
          <option value="" disabled selected>Tipo:</option>
          <option value="income">Renda</option>
          <option value="expense">Despesa</option>
        </select>
      </div>
      <div>
        <textarea
          name=""
          id=""
          rows="4"
          placeholder="Descrição"
          v-model="createInfo.description"
        ></textarea>
      </div>
      <div>
        <button @click="createTransaction">Criar</button>
      </div>
    </form>
  </div>
</template>

<style>
.create-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  padding: 5%;

  form {
    display: flex;
    border: 2px solid #9963f7;
    border-radius: 1rem;
    background-color: #f7f7f7;
    flex-direction: column;
    gap: 1rem;
    width: 100%;
    padding: 5%;

    h2 {
      color: black;
    }

    div {
      display: flex;
      gap: 0.5rem;

      input,
      select,
      textarea {
        padding: 0.5rem;
        border: 1px solid #ccc;
        border-radius: 0.3rem;
      }

      input {
        width: 70%;
      }

      select {
        width: 30%;
      }

      textarea {
        width: 100%;
      }
    }

    button {
      padding: 0.5rem;
      background-color: #9963f7;
      color: #fff;
      border: none;
      cursor: pointer;
      width: 100%;
      height: 3rem;
    }

    button:hover {
      background-color: #ededed;
      color: #9963f7;
      font-weight: 700;
      transition: 0.5s;
    }

    button:not(:hover) {
      transition: background-color 0.3s;
    }
  }
}
</style>
